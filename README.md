# Read your favourite blog on your Kindle

blog2kindle is a little Python script that can turn RSS feeds of your favourite websites (e.g. blogs, news articles) into ebooks which can be read on ebook readers, such as Kindle, Apple Books.

It's based on code from [news2kindle](https://github.com/goooooouwa/news2kindle) which reads a list of RSS feeds, package them as a MOBI file, and then send it to your kindle via kindle mail address and Amazon's whispersync. 

## Demo: ebook for blog [Coding Horror](https://blog.codinghorror.com/)

![Screen Shot 2021-11-25 at 6 49 02 PM](https://user-images.githubusercontent.com/1495607/143427994-5eea1a37-2b73-4c71-9858-eed10ea09abd.png)

![Screen Shot 2021-11-25 at 6 48 48 PM](https://user-images.githubusercontent.com/1495607/143427976-0fa33f81-93e6-4271-8562-53cce35bd1e1.png)

## Usage

`python3 ./src/news2kindle.py [blog title] [slice number]`

## Preparation

### 1. Generate RSS feeds and publish as public URLs

You can use [blog_downloader](https://github.com/goooooouwa/blog_downloader) to crawl any website and generate RSS feeds from them. See how it works [here](https://github.com/goooooouwa/blog_downloader/blob/master/README.md). 

### 2. Publish the generated RSS feeds online (as news2kindle reads content of RSS feeds from a list of URLs)

For example, you can publish the RSS feed to a Github repo (like [this one](https://github.com/goooooouwa/rss-feeds/tree/master/codinghorror)):

```bash
git add ./out/rss.xml
git commit -m "publish blog feeds"
git push origin master   # publish the RSS file somewhere online to get a public URL
```

### 3. Save public URLs of RSS feeds into `config` folder as `slice-[0-9].txt` for new2kindle to read

See examples of slice-[0-9].txt files [here](https://github.com/goooooouwa/blog2kindle/blob/master/config).

## Run

### 4. Setup environment variables from `.env` file

`export $(cat .env | xargs)`

### 5. Generate ebook from RSS feed and send it to Kindle

```
python3 ./src/news2kindle.py "blog title" 0 # which fetches the content of each RSS feeds linked in config/slice-0.txt, package them as a MOBI file, and then send it to your kindle via kindle mail address and Amazon's whispersync.
```

To generate multiple books in batch, you can run:

```
for i in {0..9}
do
echo "https://raw.githubusercontent.com/goooooouwa/out/master/out/slice-$i.xml" > config/slice-$i.txt
python3 ./src/news2kindle.py "blog title" $i
done
```

Now you will have your favourite blog sent to your Kindle, waiting for you to pick up.
