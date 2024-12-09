## headタグについて
### doctype
`<!DOCTYPE html>`
HTML5を使う
メールクライアントによっては異なるdoctypeを課したり、削除したりするものもあるが、ほとんどの場合、HTML5のdoctypeを使用することになる。（すべてのHTML5要素がサポートされているわけではない）

### HTML Tag
`<html lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:o="urn:schemas-microsoft-com:office:office">`
XMLとOOXML（Office Open XML）の名前空間を定義
OOXML=officeのXML?必要ないかも

### Charset Meta Tag
`<meta charset="utf-8">`
メールのUnicode文字エンコーディングを定義し、一般的にほとんどの言語の文字をカバー。

### Viewport Meta Tag
`<meta name="viewport" content="width=device-width,initial-scale=1">`
電子メールの表示領域をデバイスの画面の幅とし、初期ズームを100％に指定。

### Apple Scaling Meta Tag
`<meta name="x-apple-disable-message-reformatting">`
iOSデバイス上で、アップル社によるメールの不要な拡大縮小を防ぐ。
使ってない

### IE9 Meta Tag
`<meta http-equiv="X-UA-Compatible" content="IE=edge" />`
Internet Explorer 9以下のレンダリングを改善するために使用。過去にはWindows Phoneでのレンダリングを改善するためにも使われていた。
このタグは<!--[if !mso]><!-->と<!--<![endif]-->の間に置かれ、Windows Live Mailから隠れるようになっている。
？

### Title Tags
`<title></title>`
タイトルは空のままにしておくのがベスト。
受信トレイのプレビューでメールの件名の直後にこのタイトルを表示するアプリやメール送信プラットフォームに出くわすことがあるが、これは理想的ではない。

### If MSO
<!--[if mso]>と<!--[endif]-->の間に、Microsoft Outlookでのみ適用されるスタイルを定義。 border-spacing:0;（テーブル要素でcellspacing="0 "を使用するのと同等のCSS）を使用して、テーブルのギャップとセルパディングを防ぐ。 すべての div 要素のマージンをゼロにし、!.important を使用。
そうしないと、Outlook はレイアウトに多くの余分な、予測できない、不要なスペースを追加してしまう。

### XML Tag
？
`<o:OfficeDocumentSettings>`
XMLタグがあり、その中にOOXML要素<o:OfficeDocumentSettings>がある。 この設定により、Microsoft Outlookは、高DPIディスプレイ用にWindowsの設定で設定された画面の拡大率に適応するように、電子メール内のすべてのものを常に正確に変換。 これはすべて`<noscript>`タグの中にあるので、T-Onlineは "96 "を表示しない

### Inline CSS
`<style>`タグがない。 これは、Gmailアプリの非Gmailアカウントがメールの`<head>`を削除しなくなるまでは、メールのベストプラクティス。 メディアクエリをサポートしているクライアントのために、最後にメディアクエリを追加。

### Body Tag with Basic Styles
？
`<body style="margin:0;padding:0;word-spacing:normal;background-color:#ffffff;">`は、
bodyタグにいくつかの基本的なスタイルを設定。 最も重要なのはword-spacing:normal、追加しないとGmailはbody上でword-spacingを1pxに設定し、カラム間に1pxのギャップを追加してしまい、カラムが重なってしまう。


### Wrapper Div
`<div role="article" aria-roledescription="email" lang="en" style="-webkit-text-size-adjust:100%;-ms-text-size-adjust:100%;background-color:#ffffff;">`
ラッパーにはメインコンテンツが含まれ、テキストの正しいサイズを確保するためのCSSが設定され、言語（ここでは英語）が設定れる。
また、主要なメールエリアの役割を記事として定義し、aria-roledescription="email "で少し具体的にすることで、メールをアクセシブルにするための設定も含まれる。
これは、スクリーンリーダがそれがEメールであること、少なくとも記事であることを知らせるためである。
そして、このレイアウトは可能な限りテーブルではなくdivを使用するが、Eメールの本体を中央に配置するために、大きなラッパー<table>を1つ含む。
これは、特にComcastのウェブメール（米国）とLiberoのウェブメール（イタリア）のために必要。
スクリーン・リーダーが行や列の存在や構造を知らせず、代わりに内部のコンテンツだけを知らせるように、純粋にレイアウトのために使用されるテーブルの役割は、常にプレゼンテーションに設定する必要がある。

### アウターコンテナの作成
まず最初に、外側のコンテナを追加する必要がある。
WindowsのOutlookと、それ以外のすべてのメールクライアントに別々に対応する必要があるため、実際には2つのコンテナを追加することになる。
これは、メールの大部分が、CSSのmax-widthを使用して寸法を設定し、CSSのdisplay:inline-block;を使用して複数の列を横に並べて配置する`<div>`を使用して作成されるためである。
残念なことに、WindowsのMicrosoft Outlookはそのいずれにも適切に対応していないため、昔ながらのテーブルを使用する必要がある。
しかし、このような制限のあるテーブルレイアウトはOutlookだけに適用させたいので、他のメールクライアントから隠す条件付き[if mso]コメントの中にコードを入れ子にする。
このようにOutlook用に設定されたテーブルは、ゴーストテーブルとして知られている。

### シングルカラムレイアウトの場合はテーブルの方が良い

### image
Outlook用のHTMLで画像の幅を`width="640"`に設定し、CSSで`style="width:100%;height:auto; "`を設定することで、他のクライアントでも画像の幅が100%になり、常にプロポーションが保たれるようになる。


