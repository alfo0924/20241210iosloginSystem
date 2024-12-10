
import SwiftUI
import KeychainSwift

struct ContentView: View {
    @State private var isAuthenticated = false
    @State private var isRegistering = false
    @State private var username = ""
    @State private var password = ""
    @State private var errorMessage = ""
    @State private var showError = false
    
    let keychain = KeychainSwift()
    
    var body: some View {
        NavigationView {
            if isAuthenticated {
                // 登入後的主畫面
                VStack {
                    Text("歡迎回來，\(username)！")
                        .font(.title)
                        .padding()
                    
                    Button("登出") {
                        logout()
                    }
                    .foregroundColor(.white)
                    .frame(width: 200, height: 40)
                    .background(Color.red)
                    .cornerRadius(8)
                    .padding()
                }
            } else if isRegistering {
                // 註冊畫面
                VStack(spacing: 20) {
                    Text("註冊新帳號")
                        .font(.title)
                        .padding()
                    
                    TextField("使用者名稱", text: $username)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                        .padding(.horizontal)
                    
                    SecureField("密碼", text: $password)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                        .padding(.horizontal)
                    
                    Button("註冊") {
                        register()
                    }
                    .foregroundColor(.white)
                    .frame(width: 200, height: 40)
                    .background(Color.blue)
                    .cornerRadius(8)
                    
                    Button("返回登入") {
                        isRegistering = false
                    }
                    .foregroundColor(.blue)
                }
            } else {
                // 登入畫面
                VStack(spacing: 20) {
                    Text("登入")
                        .font(.title)
                        .padding()
                    TextField("使用者名稱", text: $username)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                        .padding(.horizontal)
                    
                    SecureField("密碼", text: $password)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                        .padding(.horizontal)
                    
                    Button("登入") {
                        login()
                    }
                    .foregroundColor(.white)
                    .frame(width: 200, height: 40)
                    .background(Color.blue)
                    .cornerRadius(8)
                    
                    Button("註冊新帳號") {
                        isRegistering = true
                    }
                    .foregroundColor(.blue)
                }
            }
        }
        .alert("錯誤", isPresented: $showError) {
            Button("確定", role: .cancel) {}
        } message: {
            Text(errorMessage)
        }
    }
    
    // 登入功能
    private func login() {
        guard !username.isEmpty, !password.isEmpty else {
            errorMessage = "請輸入使用者名稱和密碼"
            showError = true
            return
        }
        
        // 從 Keychain 獲取儲存的密碼
        if let storedPassword = keychain.get("\(username)_password") {
            if password == storedPassword {
                isAuthenticated = true
            } else {
                errorMessage = "密碼錯誤"
                showError = true
            }
        } else {
            errorMessage = "使用者不存在"
            showError = true
        }
    }
    
    // 註冊功能
    private func register() {
        guard !username.isEmpty, !password.isEmpty else {
            errorMessage = "請輸入使用者名稱和密碼"
            showError = true
            return
        }
        
        // 檢查使用者是否已存在
        if keychain.get("\(username)_password") != nil {
            errorMessage = "使用者名稱已被使用"
            showError = true
            return
        }
        // 儲存新用戶資料到 Keychain
        keychain.set(password, forKey: "\(username)_password")
        
        // 儲存用戶其他資訊（如果需要）
        keychain.set(username, forKey: "\(username)_profile")
        
        // 註冊成功後自動登入
        isAuthenticated = true
        isRegistering = false
        
        // 清空錯誤訊息
        errorMessage = ""
    }
    
    // 登出功能
    private func logout() {
        isAuthenticated = false
        username = ""
        password = ""
    }
}

// 預覽程式碼
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

// 自定義的使用者資料結構
struct UserProfile: Codable {
    let username: String
    let createdDate: Date
    
    init(username: String) {
        self.username = username
        self.createdDate = Date()
    }
}

// 擴展 KeychainSwift 以支援 Codable 物件
extension KeychainSwift {
    func setCodable<T: Codable>(_ value: T, forKey: String) {
        if let data = try? JSONEncoder().encode(value) {
            set(data, forKey: forKey, withAccess: .accessibleAfterFirstUnlock)
        }
    }
    
    func getCodable<T: Codable>(_ type: T.Type, forKey: String) -> T? {
        if let data = getData(forKey),
           let value = try? JSONDecoder().decode(type, from: data) {
            return value
        }
        return nil
    }
}
