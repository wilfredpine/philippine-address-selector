# Address Selector - Philippines
### PH Address JSON file from https://github.com/isaacdarcilla/philippine-addresses

![This is an image](https://lh3.googleusercontent.com/pw/AM-JKLU7NduoFSLoDW5Nz6l33o8-xRkynZIVAxuFjCYkKKmgtrTHxSSVz3gISZEsn9lb0OGG4MqIo0xAkygD0-5d0CV9VOpaqifVgEFgxc5k8ZYG3aiFvpLtUgq9XF69ONIwhKu9psfNKxDg8-XT4uf474DA=w909-h549-no?authuser=0)

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

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Address Selector - Philippines</title>
    <!--Bootstrap-->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <!--JQuery-->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body class="p-5">
    </form method="POST" action="">
        <div class="col-sm-6">
            <h3>Address Selector - Philippines</h3>
        </div>
        <hr>
        <div class="col-sm-6 mb-3">
            <label class="form-label">Region *</label>
            <select name="region" class="form-control form-control-md" id="region"></select>
            <input type="hidden" class="form-control form-control-md" name="region_text" id="region-text" required>
        </div>
        <div class="col-sm-6 mb-3">
            <label class="form-label">Province *</label>
            <select name="province" class="form-control form-control-md" id="province"></select>
            <input type="hidden" class="form-control form-control-md" name="province_text" id="province-text" required>
        </div>
        <div class="col-sm-6 mb-3">
            <label class="form-label">City / Municipality *</label>
            <select name="city" class="form-control form-control-md" id="city"></select>
            <input type="hidden" class="form-control form-control-md" name="city_text" id="city-text" required>
        </div>
        <div class="col-sm-6 mb-3">
            <label class="form-label">Barangay *</label>
            <select name="barangay" class="form-control form-control-md" id="barangay"></select>
            <input type="hidden" class="form-control form-control-md" name="barangay_text" id="barangay-text" required>
        </div>
        <div class="col-md-6 mb-3">
            <label for="street-text" class="form-label">Street (Optional)</label>
            <input type="text" class="form-control form-control-md" name="street_text" id="street-text">
        </div>
        <div class="col-md-6">
            <input type="submit" class="btn btn-success" name="submit">
        </div>
    </form>
</body>
</html>

<script>

    var my_handlers = {
        // fill province
        fill_provinces: function () {
            //selected region
            var region_code = $(this).val();

            // set selected text to input
            var region_text = $(this).find("option:selected").text();
            let region_input = $('#region-text');
            region_input.val(region_text);
            //clear province & city & barangay input
            $('#province-text').val('');
            $('#city-text').val('');
            $('#barangay-text').val('');

            //province
            let dropdown = $('#province');
            dropdown.empty();
            dropdown.append('<option selected="true" disabled>Choose State/Province</option>');
            dropdown.prop('selectedIndex', 0);

            //city
            let city = $('#city');
            city.empty();
            city.append('<option selected="true" disabled></option>');
            city.prop('selectedIndex', 0);

            //barangay
            let barangay = $('#barangay');
            barangay.empty();
            barangay.append('<option selected="true" disabled></option>');
            barangay.prop('selectedIndex', 0);

            // filter & fill
            var url = 'ph-json/province.json';
            $.getJSON(url, function (data) {
                var result = data.filter(function (value) {
                    return value.region_code == region_code;
                });

                result.sort(function (a, b) {
                    return a.province_name.localeCompare(b.province_name);
                });

                $.each(result, function (key, entry) {
                    dropdown.append($('<option></option>').attr('value', entry.province_code).text(entry.province_name));
                })

            });
        },
        // fill city
        fill_cities: function () {
            //selected province
            var province_code = $(this).val();

            // set selected text to input
            var province_text = $(this).find("option:selected").text();
            let province_input = $('#province-text');
            province_input.val(province_text);
            //clear city & barangay input
            $('#city-text').val('');
            $('#barangay-text').val('');

            //city
            let dropdown = $('#city');
            dropdown.empty();
            dropdown.append('<option selected="true" disabled>Choose city/municipality</option>');
            dropdown.prop('selectedIndex', 0);

            //barangay
            let barangay = $('#barangay');
            barangay.empty();
            barangay.append('<option selected="true" disabled></option>');
            barangay.prop('selectedIndex', 0);

            // filter & fill
            var url = 'ph-json/city.json';
            $.getJSON(url, function (data) {
                var result = data.filter(function (value) {
                    return value.province_code == province_code;
                });

                result.sort(function (a, b) {
                    return a.city_name.localeCompare(b.city_name);
                });

                $.each(result, function (key, entry) {
                    dropdown.append($('<option></option>').attr('value', entry.city_code).text(entry.city_name));
                })

            });
        },
        // fill barangay
        fill_barangays: function () {
            // selected barangay
            var city_code = $(this).val();

            // set selected text to input
            var city_text = $(this).find("option:selected").text();
            let city_input = $('#city-text');
            city_input.val(city_text);
            //clear barangay input
            $('#barangay-text').val('');

            // barangay
            let dropdown = $('#barangay');
            dropdown.empty();
            dropdown.append('<option selected="true" disabled>Choose barangay</option>');
            dropdown.prop('selectedIndex', 0);

            // filter & Fill
            var url = 'ph-json/barangay.json';
            $.getJSON(url, function (data) {
                var result = data.filter(function (value) {
                    return value.city_code == city_code;
                });

                result.sort(function (a, b) {
                    return a.brgy_name.localeCompare(b.brgy_name);
                });

                $.each(result, function (key, entry) {
                    dropdown.append($('<option></option>').attr('value', entry.brgy_code).text(entry.brgy_name));
                })

            });
        },

        onchange_barangay: function () {
            // set selected text to input
            var barangay_text = $(this).find("option:selected").text();
            let barangay_input = $('#barangay-text');
            barangay_input.val(barangay_text);
        },

    };


    $(function () {
        // events
        $('#region').on('change', my_handlers.fill_provinces);
        $('#province').on('change', my_handlers.fill_cities);
        $('#city').on('change', my_handlers.fill_barangays);
        $('#barangay').on('change', my_handlers.onchange_barangay);

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

    });


</script>
```
