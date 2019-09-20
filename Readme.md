## 座標変換、画像の回転はどうやる？

以下はPlaygroundで動かす事が出来るコード。

```swift
import UIKit
extension UIImage {
  //上下反転
  func flipVertical() -> UIImage {
    let scale: CGFloat = 1.0
    UIGraphicsBeginImageContextWithOptions(size, false, scale)
//    let imageRef = self.cgImage
    let context = UIGraphicsGetCurrentContext()!
    // Draw a red line
    context.setLineWidth(20.0)
    context.setStrokeColor(UIColor.blue.cgColor)
    context.move(to: CGPoint(x: 100, y: 100))
    context.addLine(to: CGPoint(x: 200, y: 200))
    context.strokePath()
    let imageRef = self.cgImage
    
    context.translateBy(x: 0, y:  0)
    context.scaleBy(x: 1.0, y: 1.0)
    context.draw(imageRef!, in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
    let flipHorizontalImage = UIGraphicsGetImageFromCurrentImageContext()
    UIGraphicsEndImageContext()
    return flipHorizontalImage!
  }
  
  //左右反転
  func flipHorizontal() -> UIImage {
    let scale: CGFloat = 1.0
    UIGraphicsBeginImageContextWithOptions(size, false, scale)
    let imageRef = self.cgImage
    let context = UIGraphicsGetCurrentContext()
    context?.translateBy(x: size.width, y:  size.height)
    context?.scaleBy(x: -1.0, y: -1.0)
    context?.draw(imageRef!, in: CGRect(x: 0, y: 0, width: size.width, height: size.height))
    let flipHorizontalImage = UIGraphicsGetImageFromCurrentImageContext()
    UIGraphicsEndImageContext()
    return flipHorizontalImage!
  }
}
```
ここまでは、UIImageへのMonkey Patching
```swift
UIGraphicsBeginImageContext(CGSize(width: 400, height: 300))
UIColor.red.set()
UIRectFill(CGRect(x: 10, y: 20, width: 300, height: 100))
//context!.scaleBy(x: 1.0, y: -1.0)
var image = UIGraphicsGetImageFromCurrentImageContext()
image?.flipVertical()

UIGraphicsEndImageContext()

```

こうすりゃ、画像がこうなる。image2に線を引いて、image 1を引っ繰り返して、貼り付け(draw)すれば下図の様になる。

作製: 2019/09/19Th
![flipImage](image/flipImage.png =300x300)

これで行けるはずなんだが。

[画像をリサイズ](https://gist.github.com/uupaa/f77d2bcf4dc7a294d109)

<img src="image/flipImage.png" alt="flipImage" width="33%"  />

## gitコマンド

次のコマンドで、localのcommitをremote(Github)へupload出来る。

```bash
git -u push origin master # originってのはremoteの別名
```

さて、localで作業したファイルをremoteへuploadしたら、

```
> git push -u origin master
To https://github.com/externvoid/flipPlayground.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/externvoid/flipPlayground.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

これはどう言う事なのか？

```bash
> git br -a
* master
  remotes/origin/master
```

```bash
>git pull #fetch origin/master & merge
>vi #conflict resolved
>git cm -a
>git push
```
origin/masterってのがupstream(上流ブランチ)って事らしい。
