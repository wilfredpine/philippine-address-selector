# Address Selector - Philippines
### PH Address JSON file from https://github.com/isaacdarcilla/philippine-addresses

![This is an image](https://lh3.googleusercontent.com/pw/AM-JKLU7NduoFSLoDW5Nz6l33o8-xRkynZIVAxuFjCYkKKmgtrTHxSSVz3gISZEsn9lb0OGG4MqIo0xAkygD0-5d0CV9VOpaqifVgEFgxc5k8ZYG3aiFvpLtUgq9XF69ONIwhKu9psfNKxDg8-XT4uf474DA=w909-h549-no?authuser=0)

### PSA PSGC
https://www.psa.gov.ph/classification/psgc

### Requirements:
- JQUERY

```html
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```

- HTML Tags

```html
    <select id="region"></select>
    <input type="hidden" name="region_text" id="region-text">

    <select id="province"></select>
    <input type="hidden" name="province_text" id="province-text">

    <select id="city"></select>
    <input type="hidden" name="city_text" id="city-text">

    <select id="barangay"></select>
    <input type="hidden" name="barangay_text" id="barangay-text">
```

- Javascript

Load Region
```javascript
    // load region
    let dropdown = $('#region');
    dropdown.empty();
    dropdown.append('<option selected="true" disabled>Choose Region</option>');
    dropdown.prop('selectedIndex', 0);
    const url = 'ph-json/region.json';
    // Populate dropdown with list of regions
    $.getJSON(url, function (data) {
        $.each(data, function (key, entry) {
            dropdown.append($('<option></option>').attr('value', entry.region_code).text(entry.region_name));
        })
    });
```

Change or Select Region
```javascript
// load provinces
$('#region').on('change', my_handlers.fill_provinces);
```

Load Province Function
```javascript
  function(){
    //selected region
    var region_code = $(this).val();

    // set selected text to input
    var region_text = $(this).find("option:selected").text();
    let region_input = $('#region-text');
    region_input.val(region_text);
    
    //province
    let dropdown = $('#province');
    dropdown.empty();
    dropdown.append('<option selected="true" disabled>Choose State/Province</option>');
    dropdown.prop('selectedIndex', 0);

    // filter & fill
    var url = 'ph-json/province.json';
    $.getJSON(url, function(data) {
        var result = data.filter(function(value){
            return value.region_code == region_code;
        });

        result.sort(function (a, b) {
            return a.province_name.localeCompare(b.province_name);
        });

        $.each(result, function (key, entry) {
            dropdown.append($('<option></option>').attr('value', entry.province_code).text(entry.province_name));
        })

    });
  }
```

### Full Code

#### HTML
https://github.com/wilfredpine/philippine-address-selector/blob/main/index.html

#### JS
https://github.com/wilfredpine/philippine-address-selector/blob/main/ph-address-selector.js


