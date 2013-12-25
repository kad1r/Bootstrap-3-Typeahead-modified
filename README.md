Bootstrap-3-Typeahead-modified
==============================

The Typeahead plugin from Twitter's Bootstrap 2 to use with Bootstrap 3 (with a few changes to to the original).
The version I used to extend this from can be found here: https://github.com/bassjobsen/Bootstrap-3-Typeahead

The main difference is that this version works with a list of objects working with a name/value pair (where as the original worked with a value only).

** Please note that I only made the changes to the existing code and did not rewrite everything. So don't be harsh when you judge code quality as it might have been there before :)

The code works as a autocomplete and ultimately exposes for you a itemText and itemValue variables to use as you want outside of the textbox getting selected.

I have modified the original version to fire a "selected" method that you can implement (it's optional) if you want the selected result to pass through your code and you want to use the id of the selected item.


How It Works:


After including the script file in your document, you can use the code as follows:



Calling it from your client-side code:

<script type="text/javascript" language="javascript">
    $(function () {

        var limit = 10;
        $('.js-typeahead').typeahead({
            items: limit,
            source: function (query, process) {
                return $.get('/CMS/Countries/LookupCountry', { q: query, limit: limit }, function (data) {
                    return process(data);
                });
            },
            selected: function (itemText, itemValue) {
              
              // use itemText and itemValue here to call other methods in your code
              
              return itemText; // important for script execution to continue
            }
        });
    });
</script>


Server Side (.NET - although this can be done with any server technology):

A sample of a .NET MVC Controller Action to get your data:


<code>[HttpGet]
public JsonResult LookupCountry(string q, string limit)
{
  if (limit == "")
    limit = "10";
    
  var tags = db.Countries.Where(c => c.DisplayName.Contains(q))
                .Select(c => new
                  {
                      ItemValue = c.CountryId,
                      ItemText = c.DisplayName
                  }).OrderBy(o => o.ItemText).ToList();
                  
  return Json(tags, JsonRequestBehavior.AllowGet);
}
</code>
