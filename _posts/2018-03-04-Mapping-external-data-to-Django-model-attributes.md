---
title: Mapping external data to Django model attributes
---

This post shows how to map custom pandas data frame or CSV/Excel columns to Django model attributes.

This is a very specific problem that I faced as part of a group project for a student analytics platform. I couldn’t find any direct resources to handle this and so this is my attempt to help out anyone else with this same problem. It might be easier to go through the entire code at the bottom of the page to get a better sense of the problem.

I needed to map columns from a CSV or Excel file to attributes in our Django model. The specific problem was to allow a school admin to easily enter student data via an Excel/CSV file upload. However, different schools may have different names for such attributes and there is a need for mapping.

For the complete Django view check the bottom of the page.

Firstly let’s obtain a list of the actual model attributes in our view:

<script src="https://gist.github.com/pixelexel/3ca242360de99e4b9a9699eefd94d409.js"></script>

NOTE: For simplicity convert your Excel files to CSV like so:

<script src="https://gist.github.com/pixelexel/4ea008b427fa0fa6e84af117fabace5b.js"></script>

- A GET request is used to get the path for our file. It can be used like so from another view return redirect(‘/api/fieldmatching?df=’+ path_name)

- “names” is the list of columns from the CSV file. These are what are to be mapped to the “fields”/attributes of our Django model.

- “fieldmatching” is the name of our view and HTML file.

- fields are the concerned model’s attributes.

Next in our fieldmatching.html we can list out our actual field names like so:

<script src="https://gist.github.com/pixelexel/b9ce9e452e37a82bba2001f4cdb87fc4.js"></script>

We need to allow the user to select one of these options that matches the of of the “names” to one of the actual “fields”.

<script src="https://gist.github.com/pixelexel/cdb9fe038aec9587cc8acfc9c4ef3a34.js"></script>

This should display the ‘name’ of each column from the CSV file and a set of the model attributes as options. The idea is for the user to select the attribute which most closely matches the user’s uploaded file’s column.

The hidden input allows the file’s path to be transferred to the fieldmatching view when the POST request is made.

Going back to our view, we need to change our CSV file to have the same column names as our actual field attribute names. We can do this with a post request.

<script src="https://gist.github.com/pixelexel/3e840c1daaf1e1faac849567b77eb9e4.js"></script>

Setup the path to the CSV file, create a pandas data frame and load the column list as “names” as above.

<script src="https://gist.github.com/pixelexel/cb038f9c7faf78e28db1feef15c37280.js"></script>

Now we create a dictionary called “matched”. request.POST.get() gets the attribute name corresponding to each column name. Using pandas rename() function, we rename the old columns (‘names’) with the new columns (‘fields’).

Now time to fill in the model data by converting the data frame to a dictionary and iterating:

<script src="https://gist.github.com/pixelexel/66e535bdb7b74b5c9e9d888a612911c0.js"></script>

This should allow you to create objects with custom CSV/Excel files.

Your view should look like:

<script src="https://gist.github.com/pixelexel/26f40c9408f72c1b1fdb1dedd8ea7507.js"></script>

The fieldmatching.html should look like:

<script src="https://gist.github.com/pixelexel/3c4f278424880581179f61d3613bb5c7.js"></script>

urls.py should have the following path added:

<script src="https://gist.github.com/pixelexel/6deb95b089295799b15b4be6b3560d07.js"></script>

And that’s it! Feel free to let me know how I can improve my post and let me know if you run into any problem!
