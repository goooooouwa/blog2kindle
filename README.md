# Read your favourite blogs on your Kindle

blog2kindle is a little Python script that can turn RSS feeds of your favourite websites (e.g. blogs, news articles) into ebooks which can be read on ebook readers, such as Kindle, Apple Books.

It's based on code from [news2kindle](https://github.com/goooooouwa/news2kindle) which reads a list of RSS feeds, package them as a MOBI file, and then send it to your kindle via kindle mail address and Amazon's whispersync.

## Demo: ePub version of blog [Coding Horror](https://blog.codinghorror.com/)

![Screen Shot 2021-11-25 at 6 49 02 PM](https://user-images.githubusercontent.com/1495607/143427994-5eea1a37-2b73-4c71-9858-eed10ea09abd.png)

![Screen Shot 2021-11-25 at 6 48 48 PM](https://user-images.githubusercontent.com/1495607/143427976-0fa33f81-93e6-4271-8562-53cce35bd1e1.png)

## Prerequsites:

1. Python 3.8.20 (recommend install with [pyenv](https://github.com/pyenv/pyenv))
2. ~~Calibre which provides 'ebook-convert' command (no longer necessary as Amazon now accpets sending ePub instead of Mobi files)~~

## Setup

```bash
python -m venv path-to-venv
source path-to-venv/bin/activate
python -m pip install -r requirements.txt
```

## Usage

`python3 ./src/news2kindle.py [feeds file]`

For example:

`python3 ./src/news2kindle.py config/feeds.txt`

## RSS feeds preparation

### 1. Generate RSS feeds and publish as public URLs

You can use [blog_crawler](https://github.com/goooooouwa/blog_crawler) to crawl any website and generate RSS feeds from them. See how it works [here](https://github.com/goooooouwa/blog_crawler/blob/master/README.md).

### 2. Publish the generated RSS feeds online (as news2kindle requires a list of URLs to retrieve content from RSS feeds)

For example, you can publish the RSS feed to a Github repo (like [this one](https://github.com/goooooouwa/rss-feeds/tree/master/codinghorror)):

```bash
git add ./out  # folder with RSS feeds rss-[0-9].xml
git commit -m "publish blog feeds"
git push origin master   # publish the RSS files somewhere online
```

### 3. Save public URLs of RSS feeds into a `feeds.txt` under `config` folder for new2kindle to process

See an example of `feeds.txt` file [here](https://github.com/goooooouwa/rss-feeds/blob/master/codinghorror/feeds.txt). 

### 4. Create a `config.json` configuration file for the blog under `config` folder

Example `config.json` file:

```json
// config/config.json
{
  "title": "Coding Horror",
  "author": "Jeff Atwood"
}
```

Please note, this `config.json` is compatible with the Blog Crowler config file, so you can simply copy over if you have one. See an example of Blog Crowler config.json file [here](https://github.com/goooooouwa/blog_crawler?tab=readme-ov-file#1-create-custom-page-and-post-objects-along-with-configjson-for-the-website). 

### 5. Replace the `cover.png` file with a image for the book cover under `config` folder

See an example of `cover.png` [here](https://github.com/goooooouwa/blog2kindle/blob/master/config/cover.png).

## Run

### 5. Setup environment variables from `.env` file

`export $(cat .env | xargs)`

### 6. Generate ebook from RSS feed and send it to Kindle

```
python3 ./src/news2kindle.py config/feeds.txt # which fetches the content of each RSS feeds linked in config/feeds.txt, package them as a MOBI file, and then send it to your kindle via kindle mail address and Amazon's whispersync.
```

To generate multiple books in batch, you can run:

```
for i in {0..9}
do
echo "https://raw.githubusercontent.com/goooooouwa/rss-feeds/master/codinghorror/rss-$i.xml" > config/feeds-$i.txt
python3 ./src/news2kindle.py config/feeds-$i.txt
done
```

Now you will have your favourite blog sent to your Kindle, waiting for you to pick up.
