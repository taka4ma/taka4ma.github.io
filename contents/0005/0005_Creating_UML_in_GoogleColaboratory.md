# GoogleColaboratoryでPlantUMLを使ってUMLを作る

タイトルのとおり、このnotebookではGoogleColaboratory環境に[PlantUML](https://plantuml.com/ja/)をインストールして、UMLを作成します。

## PlantUMLのインストール

### 環境の確認

#### OSを確認する

インストールする前に、まずはGoogleColaboratoryのOSを確認しておきます。


```python
!cat /etc/os-release
```

    NAME="Ubuntu"
    VERSION="18.04.3 LTS (Bionic Beaver)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 18.04.3 LTS"
    VERSION_ID="18.04"
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    VERSION_CODENAME=bionic
    UBUNTU_CODENAME=bionic


コマンドの実行結果から、Ubuntu 18.04.3 LTSということがわかります。

#### Javaがインストールされているか確認する

PlantUMLの実行にはJavaが必要なので、GoogleColaboratory環境にインストールされているか確認します。


```python
!which java
```

    /usr/bin/java


どうやらインストールされているようです。ちなみにバージョンは、


```python
!java --version
```

    openjdk 11.0.6 2020-01-14
    OpenJDK Runtime Environment (build 11.0.6+10-post-Ubuntu-1ubuntu118.04.1)
    OpenJDK 64-Bit Server VM (build 11.0.6+10-post-Ubuntu-1ubuntu118.04.1, mixed mode, sharing)


OpenJDKの11.0.6のようです。

### PlantUMLをインストールする

sourceforgeでホストされているplantumlのjarファイルを`/usr/loca/bin`へインストールします


```python
!curl -L -o /usr/local/bin/plantuml.jar http://sourceforge.net/projects/plantuml/files/plantuml.jar/download
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   178  100   178    0     0   1028      0 --:--:-- --:--:-- --:--:--  1028
    100 21190  100 21190    0     0  35854      0 --:--:-- --:--:-- --:--:-- 82773
    100   313  100   313    0     0    332      0 --:--:-- --:--:-- --:--:--   332
    100 8358k  100 8358k    0     0  6187k      0  0:00:01  0:00:01 --:--:--  100M


`ls`コマンドで確認しておきます


```python
!ls -la /usr/local/bin/plant*
```

    -rw-r--r-- 1 root root 8559351 Apr 30 13:25 /usr/local/bin/plantuml.jar


インストールできました。

### graphvizをインストール

シーケンス図・アクティビティ図以外のダイアグラムを作成する場合はGraphvizも必要なので、インストールしておきます。


```python
!sudo apt-get install -y graphviz
```

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    graphviz is already the newest version (2.40.1-2).
    0 upgraded, 0 newly installed, 0 to remove and 25 not upgraded.


### IPlantUMLをインストール

GoogleColatobatoryにPlantUMLのセルマジックを追加し、インラインSVGとして生成できるように、IPlantUMLもインストールします


```python
!sudo pip install iplantuml
```

    Collecting iplantuml
      Downloading https://files.pythonhosted.org/packages/10/b9/4db9b9ce81184d1d67f82284ca6131258b32f3f69376ee88aab5f7ff60a4/IPlantUML-0.1.1.tar.gz
    Collecting plantweb
      Downloading https://files.pythonhosted.org/packages/d6/6f/9ab1a1c3e33aaa0c0931983578c09336b092c75dce777ea666d3032f756e/plantweb-1.2.1-py3-none-any.whl
    Requirement already satisfied: six in /usr/local/lib/python3.6/dist-packages (from plantweb->iplantuml) (1.12.0)
    Requirement already satisfied: docutils in /usr/local/lib/python3.6/dist-packages (from plantweb->iplantuml) (0.15.2)
    Requirement already satisfied: requests in /usr/local/lib/python3.6/dist-packages (from plantweb->iplantuml) (2.21.0)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /usr/local/lib/python3.6/dist-packages (from requests->plantweb->iplantuml) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.6/dist-packages (from requests->plantweb->iplantuml) (2020.4.5.1)
    Requirement already satisfied: urllib3<1.25,>=1.21.1 in /usr/local/lib/python3.6/dist-packages (from requests->plantweb->iplantuml) (1.24.3)
    Requirement already satisfied: idna<2.9,>=2.5 in /usr/local/lib/python3.6/dist-packages (from requests->plantweb->iplantuml) (2.8)
    Building wheels for collected packages: iplantuml
      Building wheel for iplantuml (setup.py) ... [?25l[?25hdone
      Created wheel for iplantuml: filename=IPlantUML-0.1.1-py2.py3-none-any.whl size=4897 sha256=afc5be23c60bc3f2596072cd6ef33d2658197b2fcf0e08b9f8d81da62ca9582a
      Stored in directory: /root/.cache/pip/wheels/98/e3/22/5474b6852d1717733862688fe1d1470f749f1fe7ae0d508ce7
    Successfully built iplantuml
    Installing collected packages: plantweb, iplantuml
    Successfully installed iplantuml-0.1.1 plantweb-1.2.1




## ダイアグラムをnotebookのインラインSVGとして作成する

環境が整ったので、iplantumlを使って、notebookに埋め込まれた形のUMLを作成してみます。


```python
import iplantuml
```


```python
%%plantuml
@startuml
Title シーケンス図
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml
```




![svg](output_23_0.svg)



できました。

[PlantUML言語リファレンスガイド](http://plantuml.com/ja/guide)
 に記載されている各ダイアグラムの文法に従えば、シーケンス図以外のダイアグラムも作成できます。

## テキストとダイアグラムをファイルとして作成する

PlantUMLのテキストや、そこから生成したダイアグラムをファイルとして保存したい場合には、以下のようにします。


### PantUMLのテキストファイルを作成する

wfitefileマジックコマンドを使って、PlantUMLのテキストを、`plantuml.uml`という名前で保存します。


```python
%%writefile plantuml.uml

@startuml
Title sequence diagram
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml
```

    Overwriting plantuml.uml


ファイルができているか確認します。


```python
!ls -l
```

    total 16
    -rw-r--r-- 1 root root 8171 Apr 30 13:51 plantuml.png
    -rw-r--r-- 1 root root  210 Apr 30 13:54 plantuml.uml
    drwxr-xr-x 1 root root 4096 Apr  3 16:24 sample_data


できていました。

### テキストファイルから画像を生成する

plantuml.jarを実行して、画像を生成します。


```python
!java -jar /usr/local/bin/plantuml.jar plantuml.uml
```

正常終了するとアウトプットセルには何も出力されません。

`ls`コマンドで画像ファイルができているか確認します。


```python
!ls -l
```

    total 20
    -rw-r--r-- 1 root root 9888 Apr 30 13:54 plantuml.png
    -rw-r--r-- 1 root root  210 Apr 30 13:54 plantuml.uml
    drwxr-xr-x 1 root root 4096 Apr  3 16:24 sample_data


できていました。

notebookに読み込んで確認してみます。


```python
from IPython.display import Image
Image("./plantuml.png")
```




![png](output_36_0.png)



意図した通りの図になっています。

GoogleDriveのフォルダをColaboratory環境にマウントすれば、ここで作成したファイルをGoogleDriveへ直接保存することができます。
