# Chess Archive

As a start this repository will be simply to extract all the games for a player on chess.com

## Political Correctness

```bash
git branch -m main master
git status
git push -u origin master

## go to github and change the default branch
git push origin --delete main
```

### Web References

- [How to Rename the master branch to main in Git](https://www.git-tower.com/learn/git/faq/git-rename-master-to-main/)

## Download All Games

### CLI

The following command was used to download all the games for player `piettie`.

```bash
for g in $(curl -Ls https://api.chess.com/pub/player/piettie/games/archives | jq -rc ".archives[]") ; do curl -Ls "$g" | jq -rc ".games[].pgn" ; done >> games.pgn
```

### Using Python

The following Python code was not tested but can be tried as an alternative.

```python
# chess.com API -> https://www.chess.com/news/view/published-data-api
# The API has been used to download monthly archives for a user using a Python3 program.
# This program works as of 24/09/2018

import urllib
import urllib.request

username = "magnuscarlsen" #change 
baseUrl = "https://api.chess.com/pub/player/" + username + "/games/"
archivesUrl = baseUrl + "archives"

#read the archives url and store in a list
f = urllib.request.urlopen(archivesUrl)
archives = f.read().decode("utf-8")
archives = archives.replace("{\"archives\":[\"", "\",\"")
archivesList = archives.split("\",\"" + baseUrl)
archivesList[len(archivesList)-1] = archivesList[len(archivesList)-1].rstrip("\"]}")

#download all the archives
for i in range(len(archivesList)-1):
    url = baseUrl + archivesList[i+1] + "/pgn"
    filename = archivesList[i+1].replace("/", "-")
    urllib.request.urlretrieve(url, "/Users/Magnus/Desktop/My Chess Games/" + filename + ".pgn") #change
    print(filename + ".pgn has been downloaded.")
print ("All files have been downloaded.")
```

### Web References

- [Interest in downloading all chess.com user's games as PGN](https://www.reddit.com/r/chess/comments/8d4ou3/interest_in_downloading_all_chesscom_users_games/)
- [How I downloaded all my chess.com games using Python.](https://www.reddit.com/r/chess/comments/9ifkaq/how_i_downloaded_all_my_chesscom_games_using/)