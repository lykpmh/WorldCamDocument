# WorldCam API Server
## API說明
### 帳號管理類
- **啟用帳號**
  - 功能：提供啟用帳號時，點選郵件Link後，呼叫此API啟用帳號。
  - 網址：``[GET] api/{locale}/Account/activate?token={token}``
  - 權限：允許匿名
  - 輸入：
    - [FromQuery]：token (寄發帳號確認信時產生的Token)
  - 輸出：
    - [JResponse](#JResponse)

- **重新寄發帳號確認信**
  - 功能：當一開始註冊沒收到帳號確認信時，可呼叫此API重新發送。
  - 網址：``[GET] api/{locale}/Account/resend_confirm_mail?userAccount={userAccount}``
  - 權限：允許匿名
  - 輸入：
    - ``[FromQuery]``：``userAccount``(使用者帳號)
  - 輸出：
    - [JResponse](#JResponse)

- **發送密碼重置信**
  - 功能：當使用者忘記密碼時，使用此API重新發送密碼重置信件。
  - 網址：``[GET] api/{locale}/Account/forget_password?userAccount={userAccount}``
  - 權限：允許匿名
  - 輸入：
    - ``[FromQuery]``：``userAccount``(使用者帳號)
  - 輸出：
    - [JResponse](#JResponse)

### 檔案上傳類
- **上傳聲音/音樂檔**
  - 功能：上傳專案所需之音效檔(只允許mp3)
  - 網址：``[POST] api/Audio``
  - 權限：需要授權
  - 輸入：
    - *Form submit with ``input[type=file]``*
  - 輸出：
    - [JResponse](#JResponse)<[JUploadResponse](#JUploadResponse)>

- **上傳影像檔**
  - 功能：上傳全景圖或場景內所需的影像檔(jpg/jpeg/png)
  - 網址：``[POST] api/Image``
  - 權限：需要授權
  - 輸入：
    - *Form submit with ``input[type=file]``*
  - 輸出：
    - [JResponse](#JResponse)<[JUploadResponse](#JUploadResponse)>

- **上傳影片檔**
  - 功能：上傳場景內用到的影片檔案
  - 網址：``[POST] api/Video``
  - 權限：需要授權
  - 輸入：
    - *Form submit with ``input[type=file]``*
  - 輸出：
    - [JResponse](#JResponse)<[JUploadResponse](#JUploadResponse)>

## 列舉型態
- <a name="AcctStatusEnum"></a>AcctStatusEnum (帳號啟用狀態)
    ```csharp
    public enum AcctStatusEnum
    {
        UnConfirm = 0,      //帳號還未確認信箱
        Active = 1,         //帳號已經啟用信箱
        Suspend = 2,        //帳號已被管理員Suspend
    }
    ```
- <a name="ActiveStatusEnum"></a>ActiveStatusEnum (帳號授權碼啟用狀態)
    ```csharp
    public enum ActiveStatusEnum
    {
        UnActive = 0,       //未啟動授權碼
        Active = 1,         //已啟動授權碼
        Expired = 2         //授權碼已經過期
    }
    ```
- <a name="AuthTypeEnum"></a>AuthTypeEnum (帳號登入方式)
    ```csharp
    public enum AuthTypeEnum
    {
        Custom = 0,         //一般使用帳號密碼登入
        Facebook = 2,       //使用Facebook登入
        GooglePlus = 3      //使用Google+登入
    }
    ```
- <a name="ErrorCodes"></a>ErrorCodes (錯誤代碼)
    ```csharp
    public enum ErrorCodes
    {
        OK = 0,                             //無任何錯誤
        CustomError = 1,                    //一般錯誤，請見Message錯誤訊息
        AccessTokenExpired = 2,             //存取Token過期
        InvalidParameter = 3,               //錯誤的參數
        InsufficientPermissions = 4,        //權限不足
        CaptchaError = 5,                   //Captcha錯誤
        SocialAccountNeedProductCode = 6,   //使用SSN登入需要註冊碼
        AccountUnConfirm = 7,               //帳號尚未認證
        UnknownError = 9999                 //未知的錯誤
    }
    ```
- <a name="NoticeTypeEnum"></a>NoticeTypeEnum (通知類型)
    ```csharp
    public enum NoticeTypeEnum
    {
        ProjectComment,     //專案的評論
        ObjectComment,      //物件評論
        StorageFullAlert,   //背包快滿的通知
        ExpiredAlert        //逾期通知
    }
    ```
- <a name="OrderStatus"></a>OrderStatus (訂單狀態)
    ```csharp
    public enum OrderStatus
    {
        Pending = 10,       //擱置中
        Processing = 20,    //處理中
        Completed = 30,     //已完成
        Cancelled = 40,     //已取消
        Returned = 50       //已退貨
    }
    ```
- <a name="PeriodType"></a>PeriodType (定期類型)
    ```csharp
    public enum PeriodType
    {
        Daily = 0,      //每日
        Monthly = 1,    //每月
        Yearly = 2      //每年
    }
    ```
- <a name="PPUserOrderBy"></a>PPUserOrderBy (使用者排序方式)
    ```csharp
    public enum PPUserOrderBy
    {
        UserAccount = 0,    //根據帳號
        CreateTime = 1,     //根據建立時間
        LastLoginTime = 2   //根據上次登入時間
    }
    ```
- <a name="ProductAttribute"></a>ProductAttribute (商品屬性)
    ```csharp
    public enum ProductAttribute
    {
        Buyout = 0,     //買斷式商品
        Period = 1,     //期間式商品
        Times = 2       //次數性商品
    }
    ```
- <a name="ProductType"></a>ProductType (商品類型)
    ```csharp
    public enum ProductType
    {
        DigitalGoods = 0,   //數位商品
        PhysicalGoods = 1   //實體商品
    }
    ```
- <a name="ProjectOrderBy"></a>ProjectOrderBy (專案排序方式)
    ```csharp
    public enum ProjectOrderBy
    {
        Title,          //根據專案標題排序
        CreateTime,     //根據專案建立時間排序
        ViewCount,      //根據觀看次數排序
        LikeCount,      //根據按讚次數排序
    }
    ```
- <a name="ProjectStatusEnum"></a>ProjectStatusEnum (專案狀態)
    ```csharp
    public enum ProjectStatusEnum
    {
        Closed = 0,     //已關閉
        Open =1,        //開放中
    }
    ``` 
- <a name="RoleTypeEnum"></a>RoleTypeEnum (帳號角色)
    ```csharp
    public enum RoleTypeEnum
    {
        BasicMember = 0,        //一般測試用戶
        PaidMember = 1,         //付費用戶
        EnterpriseMember = 2,   //企業用戶
        Admin = 9               //管理員
    }
    ```
- <a name="SetupActionEnum"></a>SetupActionEnum (場景內標示物件種類)
    ```csharp
    public enum SetupActionEnum
    {
        Transition = 0,     //場景轉移
        Description = 1,    //描述物件
        Text = 2,           //文字物件
        Image = 3,          //影像物件 
        Object3D = 4,       //3D物件
        Video = 5,          //影片物件
        Text3D = 6,         //3D文字物件
        Link = 7            //超連結物件
    }
    ```
- <a name="SetupIconEnum"></a>SetupIconEnum (場景內標記物件的圖示種類)
    ```csharp
    public enum SetupIconEnum
    {
        None = 0,           //無圖示
        Transition_Up,      //上
        Transition_Down,    //下
        Transition_Left,    //左
        Transition_Right,   //右
        Transition_Circle,  //圈圈
        Description,        //描述
        TextIcon,           //文字
        VideoIcon           //影像
    }
    ```
- <a name="UploadTypeEnum"></a>UploadTypeEnum (上傳種類)
    ```csharp
    public enum UploadTypeEnum
    {
        Local = 0,              //上傳到本地
        GoogleCloudStorage = 1  //上傳到GCS
    }
    ```
- <a name="VideoTypeEnum"></a>VideoTypeEnum (影片種類)
    ```csharp
    public enum VideoTypeEnum
    {
        File = 0,       //檔案
        Youtube = 1     //Youtube連結
    }
    ```

## JSON物件
使用前須注意JSON物件屬性的第一個字均需轉換為小寫。

- <a name="JResponse"></a>JResponse (主要呼叫API後回傳物件)
    ```csharp
    public class JResponse
    {
        public ErrorCodes error_code { get; set; }  //錯誤代碼
        public string message { get; set; }         //如有錯，此為錯誤訊息
    }

    public class JResponse<T> : JResponse
    {
        public T data { get; set; }      //若需要回傳資料，使用此泛型包住
    }
    ```
- <a name="JUploadResponse"></a>JUploadResponse (上傳檔案結果)
    ```csharp
    public class JUploadResponse
    {
        public string fileId { get; set; }      //上傳完成後，取得的檔案Id(GUID)
        public string fileName { get; set; }    //上傳後的黨名
        public string url { get; set; }         //上傳後的URL
        public string thumbnailUrl { get; set; }//縮圖URL
    }
    ```

## 2.0更新為3.0更新說明
1. update-database
2. 更新dabatase
   ```sql
    #將舊有啟用過的帳號(包含逾期)變更為一般帳號
    update AppUser SET RoleType=1 WHERE RoleType=0 AND ActiveStatus != 0;
   ```
3. rr
