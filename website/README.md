This folder contains the code for our website:
[https://cds-customerdata.lfenergy.org/](https://cds-customerdata.lfenergy.org/)


## How to run your own copy of this website

* On Ubuntu 24.04+ or Debian 13+, install `jekyll`.

```
sudo apt install jekyll
```

* Navigate to this website's folder in the repo.

```
cd website/
```

* Copy the specifications from the base repo folder to the website include folder.

```
cp ../specifications/cds-wg3-01.md _includes/spec_copies/placeholder_cds-wg3-01.md
```

* Run the jekyll server, which builds the website and starts serving it on https://localhost:4000/

```
jekyll serve
```

* Done!

**Note:** If you want to make changes to the specifications themselves (to make a pull request, etc.), you need to modify them in the `specifications/` folder, not the `spec_copies/` folder.
Then, you will need to re-copy them to the `spec_copies/` folder for the changes to show up on your local website.
Finally, when you're done with your changes, you need to commit changes in the `specifications/` folder and not commit any changes to the `spec_copies/` folder.
Our automated build process for the official website will automatically copy over the specifications when a new version is built.

#### Watching for changes

When doing local development of the specifications, it's nice to automate rebuilding the website when the specification changes.
The following is an example command line loop to run that watches for changes, then copies the changes to the relevant website folder.

```
# via linux terminal in the repo root directory

cp specifications/cds-wg3-01.md website/_includes/spec_copies/placeholder_cds-wg3-01.md

while true; do
    while inotifywait -e close_write specifications/cds-wg3-01.md; do
        read -p "pause" -t 0.3 || cp specifications/cds-wg3-01.md website/_includes/spec_copies/placeholder_cds-wg3-01.md;
    done;
done
```
