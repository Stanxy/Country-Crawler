# Country Crawler
This is the project for CS6322 Spring. The following is about the crawler part.

- How to visit crawled pages
- How to find page links
- How to do parsing

## 1. How to visit crawled pages

- The crawled pages is stored in

```
csgrad1:/home/011/s/sx/sxl200012/CS6322/proj/ache/crawled
```

- For each country, it has a dirctory stores all info crawled. The pages is in the data_pages directory.

```
{csgrads1:~/CS6322/proj/ache/crawled/Colombia} ls
auth_links      data_hosts    data_pages  hub_links  metrics
data_backlinks  data_monitor  data_url    id2url     seeds_scope.txt
{csgrads1:~/CS6322/proj/ache/crawled/Colombia} cd data_pages/
{csgrads1:~/CS6322/proj/ache/crawled/Colombia/data_pages} ls
en.wikipedia.org             www.bing.com        www.lonelyplanet.com
kids.nationalgeographic.com  www.britannica.com  www.state.gov
simple.wikipedia.org         www.cia.gov         www.timeanddate.com
travel.state.gov             www.columbia.com    www.tripadvisor.com
```

- Pages for each domain are stored in one directory.
- Some of the pages downloaded may not be html (files with suffix like .jpg, .json, etc), for there's no suffix control. Most of the pages on internet are RESTful style and it's hard to filter .htm or .html for all valid pages. As a result, the crawler automatically download every resource it finds on the site map.
- The name of pages are encoded urls.

```
{csgrads1:~/CS6322/proj/ache/crawled/Colombia/data_pages/www.state.gov} ls
http%3A%2F%2Fwww.state.gov%2Fj%2Fdrl%2Firf%2Frpt%2Findex.htm
http%3A%2F%2Fwww.state.gov%2Fr%2Fpa%2Fei%2Fbgn%2F35754.htm
https%3A%2F%2Fwww.state.gov%2Fabout%2F
https%3A%2F%2Fwww.state.gov%2Falberto-carrizosa-gelzis-felipe-carrizosa-gelzis-and-enrique-carrizosa-gelzis-v-republic-of-colombia%2F
...
```

- To obtain the original url, you could use the URLDecode method in Java

```
public static void main(String[] args) {
        String s = null;
        try {
            s = URLDecoder.decode("$FILE_NAME", "UTF-8");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println(s);
    }
```

## 2. How to find page links

- For in links, pleace check the "auth_links" file under country dirctory
- For out links, please check the "hub_links" file under country dirctory
- For page id to url, please check the "id2url" file under country dirctory.

* The splitor is '\t' between the start id and other items, and " " between each page id in hub_links.

```
{csgrads1:~/CS6322/proj/ache/crawled/Colombia} head -n 2 hub_links
1460	40657
1581	44252 44253 44254 44255 44256 44257 44258 44259 44260 44261 44262 44263 44264 44265 44266 44267 44268 44269 44270 44271 44272 44273 44274 44275 44276 44277 44278 44279 44280 44281 44282
{csgrads1:~/CS6322/proj/ache/crawled/Colombia} head auth_links
3640	39
3638	39
3639	39
...
{csgrads1:~/CS6322/proj/ache/crawled/Colombia} head -n 3 id2url
3640	https://www.timeanddate.com/weather/spain/barcelona
3638	https://www.timeanddate.com/weather/israel/jerusalem
3639	https://www.timeanddate.com/weather/usa/salt-lake-city
```

## 3. How to do parsing

- To parse the pages, you could use open source soup package. Here is an example of Java package jsoup-1.13.1.jar

```
public static void main(String[] args) {
        String fileContentString = $RAW_HTML_THAT_YOU_HAVE
        String s = null;
        s = parseHTML(fileContentString);
        s = s.trim();
        System.out.println(s);
    }
```

- or to read from file you can do

```
public String breakFileToLines(File file) {
        Long fileLength = file.length();
        byte [] fileContent = new byte[fileLength.intValue()];
        String fileContentString;
        try(FileInputStream inputStream = new FileInputStream(file)){

            inputStream.read(fileContent);
            fileContentString = new String(fileContent);

            fileContentString = parseHTML(fileContentString);
            fileContentString = fileContentString.trim();

            return fileContentString;
        } catch (FileNotFoundException e){
            System.out.println(file.getName());
            System.out.println("There's not such file.");
        } catch (IOException e){
            System.out.println("IO exception " + e.toString());
        }

        return null;
    }
```

