# DARTH Docs – Updating Instructions 

## 1.  Edit the docs 

### Get the project locally 

Download or clone the DARTH-docs repository to your computer: 

~~~
cd DARTH-docs
~~~

 

### All documentation content is in the docs/ folder. 

Each .md file corresponds to a page. 

Example: 

~~~
docs/index.md   ← Home page

	docs/overview.md
  
	docs/....... (the other pages of the DARTH DOCS)
~~~

### Open the Markdown file you want to update in your code editor  

edit

## 2.  Preview changes locally 

Before publishing, you can see how your changes will look in the browser: 

~~~
python3 -m mkdocs serve
~~~

- Open your browser at: http://127.0.0.1:8000/ 

- Changes you save in docs/ will automatically refresh in the browser. 

## 3. Commit changes 

Once you’re happy with edits: 

~~~
git add . 

git commit -m "Update [page name or summary]"
~~~
 

## 4. Push to GitHub 

~~~
git push
~~~

This updates the main branch with your latest Markdown edits. 

## 5. Deploy the site 

Publish the updated site to GitHub Pages: 

~~~
python3 -m mkdocs gh-deploy
~~~

This rebuilds the site/ folder and pushes it to the gh-pages branch. 

The live site will automatically update at: 

https://darth-docs.github.io/DARTH-docs/edp/
