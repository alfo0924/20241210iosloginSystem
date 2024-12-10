
## KeychainSwift
這是一個用於安全存儲數據的框架，具有以下特點：

- 提供簡單的API介面來存取和管理Keychain中的數據[1]
- 自動加密所有存儲的數據，確保數據安全性[1]
- 支持存儲字串、布林值和其他數據類型[2]
- 適用於iOS 9+等Apple平台[1]

## SwiftUI
這是用於構建使用者介面的框架：

- 採用宣告式語法來開發UI，使程式碼更簡潔直觀[3]
- 使用@State來管理視圖狀態：
  - 當數據改變時自動觸發視圖重繪
  - 建立數據源與視圖的關係[3]

## 核心工作原理

1. 資料存儲：
- 使用KeychainSwift將用戶憑證安全加密存儲在設備的Keychain中[5]
- 支持同步和異步操作，確保數據存取不影響用戶體驗[5]

2. 視圖管理：
- SwiftUI通過@State監測數據變化
- 當數據改變時自動更新對應的視圖元件[3]
- 採用單向數據流模式，使數據流轉更清晰可控[3]




需要先安裝 KeychainSwift 套件。在 Xcode 中按照以下步驟操作：

1. 選擇 File > Add Packages
2. 在搜索欄輸入：https://github.com/evgenyneu/keychain-swift.git
3. 點擊 Add Package 完成安裝

