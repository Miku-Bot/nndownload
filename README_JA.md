# nndownload

![PyPI](https://img.shields.io/pypi/v/nndownload.svg)

<p align='center'>
    <a href='./README.md'>EN</a> | JA
</p>

nndownloadでは、 [Niconico](http://nicovideo.jp)(通称ニコニコ動画)から動画、画像、マンガをダウンロードしたり、その他のリンクを処理したりできます。従来のダウンローダーとは異なり、nndownloadではDMC (Dwango Media Cluster) サーバーと通信して、高品質の動画へのアクセスを確保します。

## Disclaimers
- [プレミアムアカウント](https://secure.nicovideo.jp/secure/premium_detail/)を持っていない場合、エコノミーモードの時間帯  (通常は日本時間の午後12時～午前2 時) または混雑する時間帯では低品質の動画となります。
- ログインせずに (-g/--no-login を使用して) ダウンロードする場合、一部の動画はダウンロードできないか、低品質でしか利用できない場合があります。
- 同じ接続で複数のダウンロードセッションを実行すると、一時的なブロックやスロットリングが発生する可能性があります。
- 次の機能は現在サポートされていません。
  - ニコ生タイムシフトのダウンロード
  - ニコニコ静画のサムネイルとコメントのダウンロード
  - チャンネルやブログ、コメントのダウンロード


## Features
 - コメント、サムネイル、メタデータを含む動画のダウンロード
 - メタデータを含むニコニコ静画の画像やマンガのダウンロード
 - メタデータを含むチャンネル動画やブログのダウンロード
 - マイリストのダウンロード
 - ユーザの動画、マイリスト、イラスト、マンガのダウンロード
 - ニコ生用のストリームURLの生成
 - 複数スレッドによる動画ダウンロードの高速化
 - テキストファイルで受け取ったURLリストの処理

## Requirements
### Python version
- Python >=3.5.3

### Dependencies
- aiohttp
- aiohttp-socks
- beautifulsoup4
- requests
- mutagen
- setuptools
- urllib3

# Installation
```bash
pip install nndownload
```

Windows用のバイナリファイルは[リリースページ](https://github.com/AlexAplin/nndownload/releases)より利用できます。

## Usage
### CLI
```
usage: nndownload.py [options] input

positional arguments:
  input                 URLs or files

optional arguments:
  -h, --help            ヘルプメッセージを表示
  -u EMAIL/TEL, --username EMAIL/TEL
                        アカウントのメールアドレスor電話番号
  -p PASSWORD, --password PASSWORD
                        アカウントのパスワード
  --session-cookie COOKIE
                        session cookie
  -n, --netrc           .netrcを認証に利用
  -q, --quiet           コンソールに出力しない
  -l, --log             ファイルにログ出力
  -v, --version         プログラムのバージョン情報を表示

download options:
  -y PROXY, --proxy PROXY
                        http or socks proxy
  -o TEMPLATE, --output-path TEMPLATE
                        出力パスを指定 (テンプレートオプションを参照)
  -r N, --threads N     指定したスレッド数で動画をダウンロードする
  -g, --no-login        ログインなしでダウンロードセッションを作成
  -f, --force-high-quality
                        高品質な動画ソースが利用可能な場合のみダウンロードを行う
  -a, --add-metadata    動画ファイルにメタデータを付与 (MP4 only)
  -m, --dump-metadata   メタデータをファイルにダンプ
  -t, --download-thumbnail
                        動画のサムネイルをダウンロード
  -c, --download-comments
                        動画のコメントをダウンロード
  -e, --english         英語版サイトにリクエストする
  -aq AUDIO_QUALITY, --audio-quality AUDIO_QUALITY
                        音質を指定
  -vq VIDEO_QUALITY, --video-quality VIDEO_QUALITY
                        画質を指定
  -s, --skip-media      メディアのダウンロードをスキップ
  --playlist-start N    プレイリストの開始番号を指定 (最小値：0)
```

### Module
```python
import nndownload

url = "https://www.nicovideo.jp/watch/sm35249846"
output_path = "/tmp/{id}.{ext}"
nndownload.execute("-g", "-o", output_path, url)
```

### Custom Output Paths
カスタムファイルパスは、標準の Python テンプレート文字列のように構築されます。例：`{uploader} - {title}.{ext}`  
ニコニコ静画の場合、出力パスはチャプターディレクトリのテンプレートにする必要があります。例：`{manga_id}\{id} - {title}`  
利用可能なオプションは以下の通りです。

- comment_count (videos, images, manga, articles)
- description (videos, images, manga)
- document_url (videos, images, manga, articles)
- ext (videos, images, articles)
- id (videos, images, manga, articles)
- published (videos, images, manga, articles)
- tags (videos, images, manga, articles)
- title (videos, images, manga, articles)
- uploader (videos, images, manga, articles)
- uploader_id (videos, images, manga, articles)
- url (videos, images)
- view_count (videos, images, manga)
- audio_quality (videos)
- video_quality (videos)
- article (articles)
- blog_title (articles)
- clip_count (images)
- duration (videos)
- manga_id (manga)
- manga_title (manga)
- mylist_count (videos)
- page_count (manga)
- size_high (videos)
- size_low (videos)
- thread_id (videos)
- thread_key (videos)
- thread_params (videos)
- thumbnail_url (videos)

### Using Stream Links
ストリームURLの生成後は、ストリームをアクティブな状態に保つためにプログラムを実行し続ける必要があります。生成されたストリームURLを再生するには、[mpv](https://github.com/mpv-player/mpv)と[streamlink](https://github.com/streamlink/streamlink)が最適なオプションです。ただし、アグレッシブなHLS キャッシングとスレッドを使用する他のプログラムも動作する可能性があります。

`mpv https://...`
`streamlink https://... best`

## Known Bugs
- 既知のバグについてはopen issuesをご参照ください。

## License
This project is licensed under the MIT License.