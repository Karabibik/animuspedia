# Google Sheets Eklentileri
Oynanış videolarından izleyerek çeviri yapılan zamanlarda ya da birçok kişi dosyaya son hâlini vermek için giriştiğinde CAT programları yerine Google Sheets kullanmak daha mantıklı oluyor. Buradaki kimi eksikliği gidermek için bazı eklentiler oluşturduk.

## Terimce Eklentisi
Yalnız Google Sheets'te CAT programlarındaki Translation Memory ve Term Base sistemi olmadığından terimcesi büyük projelerde biraz yorabiliyor.

O yüzden Terim kontrolü için şöyle basit bir sistem geliştirdik, seçili cümledeki terimleri bulup yandaki ekranda gösteriyor. Nadiren hataları olmuyor değil ama yeterince hızlı ve işi fazlasıyla kolaylaştırıyor.

Aynı şeyi ortak çeviri ifadelerini bulmak için de kullanmak mümkün ama birkaç yüz bin kelimenin üstündeki dosyalarda elle arayıp bulmak çok daha hızlı.

Örnek `Javascript` kodu:
```javascript
//@OnlyCurrentDoc

// Only need to change these parts
var text_page  = "Texts_v133"
var terms_page = "Terimler Sözlüğü" // A: EN, B: TR, C: Description
var menu_title = "Animus++"

// Select the translation page
var texts = SpreadsheetApp.getActive().getSheetByName(text_page)

// Get terminology lists
var terms = SpreadsheetApp.getActive().getSheetByName(terms_page)
var max_row = terms.getLastRow().toString();

// Get all ranges, make them a list
var en_terms  = terms.getRange("A2:A"+max_row).getValues().flat();
var tr_terms  = terms.getRange("B2:B"+max_row).getValues().flat();
var term_desc = terms.getRange("C2:C"+max_row).getValues().flat();

function onOpen() {
  SpreadsheetApp
  .getUi()
  .createMenu(menu_title)
  .addItem("Terimler Sözlüğü", "showSidebarx")
  .addToUi();
}

// To show terms in sidebar
function showSidebarx() {
  var html = HtmlService.createHtmlOutputFromFile("page")
  .setTitle('Terimler Sözlüğü');
  SpreadsheetApp.getUi().showSidebar(html);
}

// Get the simple matches
function get_term_match(){

  // Get active sentence, lower case
  var sentence = texts.getActiveCell().getValue().toLowerCase();

  // Initialize matches list
  var matches = [];

  // For each word in terminology
  for (var word in en_terms){

	// Get english term
	var english = en_terms[word];

	// If the term is included in sentence as a substring
	if(sentence.includes(english.toLowerCase())){

	  // Get the translation
	  var turkish = tr_terms[word];
	  var desc = term_desc[word];

	  // Add them to list
	  matches.push(english.toString());
	  matches.push(turkish.toString());
	  matches.push(desc.toString());
	}
  }

  return matches;
}
```

Örnek `HTML` kodu:
```html
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
  </head>
	
  <!--Basic formatting-->
  <style>
    table,
    th,
    td {
      border: 1px solid black;
      border-collapse: collapse;
      padding: 7px;
    }
  </style>
	
  <body>
  
	<!--Simple button for search-->
    <p>
      <input type="button" class="button" style="width:275px; height:40px" value="Terimleri Bul" onclick="find_terms();">
    </p>
	  
    <hr>
	  
	<!--Shows the status-->
    <p id="control"></p>
	  
	<!--Shows the term matches-->
    <table id="terms">
    </table>
    
    <script>
    
      // Run the .gs function to find the matches and direct to table
      function find_terms() {
      
        // Reset the control and table
        document.getElementById("terms").innerHTML = "";
        document.getElementById("control").innerHTML = "Aranıyor";
        
        // Run the script
        google.script.run.withSuccessHandler(add_to_table).get_term_match();
        
      }

      // Display found matches as table
      function add_to_table(my_array) {
      
        // If any matches exist
        if (my_array.length > 0) {
        
          // Remove "Aranıyor"
          document.getElementById("control").innerHTML = "";
          
          // Get table by ID
          var table = document.getElementById("terms");
          
          // Create table header
          var mytable = "<tr><th>İngilizce</th><th>Türkçe</th><th>Açıklama</th></tr>";
          
          // Add matches
          var i = 0;
          for (i = 0; i < my_array.length; i += 3) {
            mytable += "<tr>";
            mytable += "<td style=\"width: 75px\">" + my_array[i]   + "</td>";
            mytable += "<td style=\"width: 75px\">" + my_array[i+1] + "</td>";
            mytable += "<td style=\"width: 125px\">"+ my_array[i+2] + "</td>";
            mytable += "</tr>";
          }
          
          // Display table
          document.getElementById("terms").innerHTML = mytable;
        } else {
          document.getElementById("control").innerHTML = "Terim bulunamadı";
        }
      }
    </script>
  </body>
</html>
```

## ModernMT Eklentisi
Trados plugini olan ModernMT'yi doğrudan Google Sheets üzerinden kullanmak da mümkün. Aşağıdaki örnek kodda API anahtarınız ile eğittiğiniz motoru girerseniz doğrudan çeviri hücresine `=MT()` yazarak makine çeviriyi alabilirsiniz.

```javascript
function MT() {
  
  // Assumes the English text is in the left-adjacent cell
  var spreadsheet = SpreadsheetApp.getActive();
  var sheet = spreadsheet.getActiveSheet();
  var cellRange = sheet.getActiveCell();
  var selectedColumn = cellRange.getColumn();
  var selectedRow = cellRange.getRow();
  var range = sheet.getRange(selectedRow,selectedColumn-1); 
  var input = range.getValue();

  // Change here for tuning
  var engine = "mk_asdfasgq324fsdfvv2341w"
  var header = {
    'MMT-ApiKey' : "AAAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE"
  };
  
  var options = {
    'method' : 'get',
    'headers' : header
  };
  
  // Make request and get the response 
  response = UrlFetchApp.fetch('https://api.modernmt.com/translate?source=en&target=tr&hints=' + engine + '&q=' + encodeURI(input), options);
  
  // Parse the response and return translation
  jobject = JSON.parse(response)
  return jobject.data.translation
  
}
```