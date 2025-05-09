# G-scripts Sheet DB/Front end for cheap but slow "websites"

These have come in handy, so this is just a template for future/shared use as I have needed these more times then I can count and having one half baked already is very helpful

Since it is all sheets and Google script, this is mainly to keep track of everything

["DB" sheet](https://docs.google.com/spreadsheets/d/1_EwuEdZr56JaZavnDyAm9YRJdtSePABi_UgLd7l1-mI/edit?gid=0#gid=0)

[Viewer](https://script.google.com/u/0/home/projects/1kyXbqCLEwl8CzjgcSnfcDDuOe_c-0Xw27eUGf2uhZh9ODW7mUzkEO-pA/edit)

[New Entries](https://script.google.com/home/projects/1THd3mzdcqkWxleeVH7N-DythOmEQiyQeYy45eYJmr5Dkvghu6JHMv438/edit)

[Specific Entry](https://script.google.com/home/projects/1CukVkiWCiBmJ7lY2-Z497inLZHamyxsG1R5CcU0MQWhKRuvZ2pXKQbOU/edit)

[Sheets Library](https://script.google.com/home/projects/1VpRuC3-W2ZsQ_NSrhe2KwtMfmBOUc-W94UexGNipwuD7tvTkvugRorrw/edit)

The sheets can be dynamically changed by simply altering the sheet itself, for example in the [data tab](https://docs.google.com/spreadsheets/d/1_EwuEdZr56JaZavnDyAm9YRJdtSePABi_UgLd7l1-mI/edit?gid=0#gid=0) one can add a new column to the left and the data pull will use it and pull the column name assigned so it can be used. However there is no link between the [fields](https://docs.google.com/spreadsheets/d/1_EwuEdZr56JaZavnDyAm9YRJdtSePABi_UgLd7l1-mI/edit?gid=1880282481#gid=1880282481) and the [data](https://docs.google.com/spreadsheets/d/1_EwuEdZr56JaZavnDyAm9YRJdtSePABi_UgLd7l1-mI/edit?gid=0#gid=0) tabs yet. I think this could be easily done, but is low on the list for improvements right now. So updating one more or less necessitates updateing the other

The sheets communicate very simply. There are some standardized libraries in the [libraries](https://script.google.com/home/projects/1VpRuC3-W2ZsQ_NSrhe2KwtMfmBOUc-W94UexGNipwuD7tvTkvugRorrw/edit) scripts that access the sheet directly (for reading and writing) access will need to be propogated. Usually this is easily done with triggering any of the functions that touch sheets. Once those are setup, add real data, or atleast real columns, so that the populating can begin. These are just bare bones and the UI reflects that. However for any website based sites like [Viewer](https://script.google.com/u/0/home/projects/1kyXbqCLEwl8CzjgcSnfcDDuOe_c-0Xw27eUGf2uhZh9ODW7mUzkEO-pA/edit) and [New Entries](https://script.google.com/home/projects/1THd3mzdcqkWxleeVH7N-DythOmEQiyQeYy45eYJmr5Dkvghu6JHMv438/edit) there is a consistent structure that looks like this:
#### Code.gs
- Manages external libraries and moves data from sheets in a JSON format (straight objects cannot be used!)
- Handles the rendering of the other pages, this can be added to or altered via requests
#### interface.html
- Manages the pure HTML elements, so Body, HTML, and other baseline tags. Additionally I leave skeletons typically here that can be filled in later by JS
#### javascript.html 
- Where the majority of the "compute" happens. This is intentional as this offloads alot of processing onto the user machine instead of googles slower cloud instances (not complaining, they are free after all)
- Handles the entire sheets object, and picks apart the pieces it needs using a "hydration" adjacent structure. I mainly say adjacent because it is really front-end that NEEDS to trigger backend, so changes are queued up on some timer or are a one-shot on page load. But this allows alot of flexibility on the incoming object. This structure is as follows:
  ```json
  {
      "sheet_name":             "Mainly for keeping track, is made from the request itself",
      "sheet_id":               "Mainly for keeping track, is made from the request itself",
      "time_requested":         "Mainly for keeping track, is generated at the time of receiving the request",
      "conversion_dictionary":  "This contains the headers in a dictionary that uses indices. For example { 'id':0, 'name':1, 'data':2,...}",
      "data":                   "Allll of the sheet data, all it filters is the headers and any rows that are marked as disabled" 
  }
  ```
#### css.html
- just styles and nothing else really. Wanna messs with the UI, do it here ideally
