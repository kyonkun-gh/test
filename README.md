# test

## SQL 四大指令分類
- **DDL（資料定義語言）**：CREATE, ALTER, DROP, TRUNCATE, RENAME, COMMENT
- **DML（資料操作語言）**：SELECT, INSERT, UPDATE, DELETE
- **DCL（資料控制語言）**：GRANT, REVOKE
- **TCL（交易控制語言）**：COMMIT, ROLLBACK, SAVEPOINT

## ANSI/SPARC 資料庫三層架構（Three-Schema Architecture）
### 這是描述資料庫系統的層次結構與抽象層級：
| 層級   | 名稱        | 說明                                   | 使用對象               |
|--------|-------------|----------------------------------------|------------------------|
| 外部層 | External    | 使用者視圖，每位使用者看到的子集合     | 使用者 / 開發者        |
| 概念層 | Conceptual  | 全企業邏輯結構，所有資料的整體模型     | 系統設計者             |
| 內部層 | Internal    | 資料實際儲存方式，例如索引與壓縮方式   | DBA（資料庫管理員）    |

## 資料庫設計三階段（Database Design Phases）
| 階段       | 名稱                         | 工具 / 方法                         | 對應三層架構      |
|------------|------------------------------|-------------------------------------|-------------------|
| 第 1 階段  | 概念設計（Conceptual Design） | ER 模型（Entity-Relationship）      | 對應「概念層」    |
| 第 2 階段  | 邏輯設計（Logical Design）    | 轉為關聯表、設計主鍵、外鍵等        | 部分對應「外部層」|
| 第 3 階段  | 實體設計（Physical Design）   | 選資料型別、建索引、效能優化等      | 對應「內部層」    |

## ER 模型屬性類型
| 屬性類型                        | 說明                     | 範例                   |
|----------------------------------|--------------------------|------------------------|
| 必要屬性 Required Attribute      | 不可為 NULL              | 姓名、學號             |
| 可選屬性 Optional Attribute      | 可為 NULL                | 傳真號碼、第二電話     |
| 單值屬性 Single-valued Attribute | 一筆資料對應一個值       | 性別、生日             |
| 多值屬性 Multi-valued Attribute  | 一筆資料可對應多個值     | 技能、電話號碼         |
| 組合屬性 Composite Attribute     | 可再拆分成多個欄位       | 地址 → 縣市 + 街道     |
| 簡單屬性 Atomic/Simple Attribute | 不可拆分                 | 姓名、郵遞區號         |
| 推論屬性 Derived Attribute       | 從其他欄位計算得出       | 年齡、工作年資         |
| 儲存屬性 Stored Attribute        | 真正存入資料庫的值       | 出生日期、薪資         |
| 識別/鍵屬性 Key Attribute        | 可唯一識別一筆記錄       | 學號、員工編號         |

## ER 模型個體型態
- **強個體型態**：可獨立存在，有主鍵
- **弱個體型態**：需依賴其他實體才能存在

## 鍵（Key）的類型
| 鍵類型                | 英文名稱         | 說明                                                                                  |
|-----------------------|------------------|---------------------------------------------------------------------------------------|
| 超鍵                  | Super Key        | 包含一組欄位，可以唯一識別資料表中的每筆資料，但可能包含多餘欄位（非最小鍵）。         |
| 候選鍵                | Candidate Key    | 具備唯一性的欄位或欄位組合，具有成為主鍵的資格。主鍵是從候選鍵中挑選出來的。           |
| 主鍵                  | Primary Key      | 唯一標識資料表中每一筆紀錄的欄位或欄位組合，不可為空值（NULL）。                       |
| 交替鍵                | Alternate Key    | 候選鍵中未被選為主鍵的其他候選鍵。                                                    |
| 組合鍵                | Composite Key    | 由多個欄位組成的鍵，用來唯一標識資料列。                                               |
| 外來鍵                | Foreign Key      | 在一個表中，用來參考另一表的主鍵，建立表間關聯並維護參考完整性。                       |
| 次鍵                  | Secondary Key    | 用於加速查詢的欄位索引，不一定唯一，不是用來唯一標識資料列。                           |
- **超鍵 > 候選鍵 = 主鍵+交替鍵**	

## 資料庫的完整性規則
1. **實體完整性規則（Entity Integrity Rule）**  
    在單一資料表中，主鍵（Primary Key）必須具有唯一性，且不可為空值（NULL）。

2. **參考完整性規則（Referential Integrity Rule）**  
    在兩個資料表中，次要資料表的外鍵（Foreign Key, FK）欄位值，必須存在於主要資料表的主鍵（Primary Key, PK）欄位值中。

3. **值域完整性規則（Domain Integrity Rule）**  
    在單一資料表中，同一資料行的屬性值必須符合預先定義的資料型別或範圍。

## ACID 四大交易特性
| 特性       | 英文名稱    | 說明                                   | 目的/效果                       |
|------------|-------------|----------------------------------------|----------------------------------|
| 原子性     | Atomicity   | 一筆交易要嘛全部完成，要嘛完全不做      | 防止交易一半成功                 |
| 一致性     | Consistency | 交易執行前後，資料要符合預設規則        | 確保資料合法性                   |
| 隔離性     | Isolation   | 多筆交易同時執行時，不互相干擾          | 避免交易交錯讀寫導致資料錯亂     |
| 持久性     | Durability  | 一旦交易完成，資料變更永久保存          | 防止資料因故障丟失               |

## 主要鎖定模式（Lock Modes）
| 鎖定類型             | 縮寫         | 說明                                                                                   |
|----------------------|--------------|----------------------------------------------------------------------------------------|
| 共用鎖               | S            | 用於讀取資料（如 SELECT），允許多個交易同時讀取，但不允許寫入。                        |
| 獨佔鎖               | X            | 用於寫入資料（如 INSERT、UPDATE、DELETE），只允許一個交易存取，其他交易無法讀寫。      |
| 更新鎖               | U            | 用於可能會更新資料的操作（例如讀完後再更新），避免死結，先取得更新鎖，再升級為獨佔鎖。|
| 意圖共用鎖           | IS           | 表示某個子物件（如列）有共用鎖，用於支援表級鎖與列級鎖之間的協調。                    |
| 意圖獨佔鎖           | IX           | 表示某個子物件有獨佔鎖，例如列被 X 鎖時，表層會加 IX 鎖。                             |
| 意圖更新鎖           | IU           | 表示有子物件正嘗試取得 U 鎖。                                                          |
| 架構鎖（Schema Lock）| Sch-S / Sch-M| 當執行 SELECT（Sch-S）或 ALTER TABLE（Sch-M）等資料結構操作時使用。                   |
| Bulk Update 鎖       | BU           | 用於大量資料匯入操作（如 BULK INSERT），允許快速載入資料。                             |

## 鎖定層級（Lock Granularity）
| 層級         | 說明                                                         |
|--------------|--------------------------------------------------------------|
| 資料列 (Row) | 最細的鎖定單位，並發性高但開銷大。                           |
| 資料頁 (Page)| 一頁 8KB，通常包含多筆資料列。                               |
| 資料表 (Table)| 對整張表加鎖，效率高但會造成阻塞。                           |
| 資料庫 (Database)| 很少用，除非執行大規模維護。                             |

## 正規化
| 正規化階段 | 要解決的問題           | 規則說明                                   |
|------------|------------------------|--------------------------------------------|
| 1NF        | 重複欄位（非原子值）   | 每個欄位都是不可再分的原子值               |
| 2NF        | 部分相依               | 非主鍵欄位必須完全依賴主鍵                 |
| 3NF        | 傳遞相依               | 非主鍵欄位不能依賴其他非主鍵欄位           |
| BCNF       | 候選鍵問題             | 所有決定欄位都必須是候選鍵                 |


## 正規化範例
原始資料表（未正規化）
| 學生編號 | 學生姓名 | 課程資料                      | 成績       |
| ---- | ---- | ------------------------- | -------- |
| S001 | 小明   | C001:國文, C002:數學, C003:英文 | 80,90,85 |
| S002 | 小華   | C001:國文, C003:英文          | 75,88    |


第一正規化（1NF）: 拆分多值欄位，讓資料具「原子性」
| 學生編號 | 學生姓名 | 課程編號 | 課程名稱 | 分數 |
|----------|----------|----------|----------|------|
| S001     | 小明     | C001     | 國文     | 80   |
| S001     | 小明     | C002     | 數學     | 90   |
| S001     | 小明     | C003     | 英文     | 85   |
| S002     | 小華     | C001     | 國文     | 75   |
| S002     | 小華     | C003     | 英文     | 88   |

第二正規化（2NF）:消除「部分依賴」，以複合主鍵為基礎拆表（學生編號 + 課程編號）

Student 資料表
| 學生編號 | 學生姓名 |
|----------|----------|
| S001     | 小明     |
| S002     | 小華     |

Score 資料表（保留課程名稱，尚未進 3NF）
| 學生編號 | 課程編號 | 課程名稱 | 分數 |
|----------|----------|----------|------|
| S001     | C001     | 國文     | 80   |
| S001     | C002     | 數學     | 90   |
| S001     | C003     | 英文     | 85   |
| S002     | C001     | 國文     | 75   |
| S002     | C003     | 英文     | 88   |

第三正規化（3NF）:消除「傳遞依賴」，進一步拆出課程表

課程名稱 僅依賴於 課程編號，不是主鍵的一部分，產生傳遞依賴 → 不符合 3NF

Student 資料表
| 學生編號 | 學生姓名 |
|----------|----------|
| S001     | 小明     |
| S002     | 小華     |

Course 資料表
| 課程編號 | 課程名稱 |
|----------|----------|
| C001     | 國文     |
| C002     | 數學     |
| C003     | 英文     |

Score 資料表
| 學生編號 | 課程編號 | 分數 |
|----------|----------|------|
| S001     | C001     | 80   |
| S001     | C002     | 90   |
| S001     | C003     | 85   |
| S002     | C001     | 75   |
| S002     | C003     | 88   |


## 資料庫系統環境通常包含以下三大要素
- **資料（Data）**		系統所管理的資料本身。
- **硬體（Hardware）**	運行資料庫系統所需的物理設備，如伺服器、儲存裝置等。
- **程序（Procedure）**	操作資料庫系統的規則、流程與方法。

## 主要用來儲存資料庫結構相關的描述性資料
- **資料庫綱要（Schema）**：資料表、欄位、資料型別、限制等結構定義
- **索引資料**：資料表上的索引結構資訊
- **視界（View）資訊**：虛擬表格定義及相關資料

## 資料庫三大模型
| 項目             | 階層式         | 網路式           | 關聯式                  |
|------------------|----------------|------------------|-------------------------|
| 結構             | 樹狀結構       | 圖狀結構         | 表格結構                |
| 關係型態         | 一對多         | 多對多           | 任意（透過外鍵）        |
| 代表關係         | 父子           | 擁有者-成員      | 主鍵-外鍵               |
| 彈性             | 低             | 中               | 高                      |
| 查詢語言         | 複雜、自定     | 複雜、自定       | 標準 SQL                |
| 是否支援多對多   | ❌             | ✅               | ✅（透過中介表）        |
| 開發維護難度     | 高             | 更高             | 相對較低                |


## Java
- **建構式**: 建立物件(new)時初始化物件狀態
- **預設建構式**: 沒有明確寫出任何建構式，編譯器預設會有一個無參數、空內容的建構式，稱為預設建構式

- **物件導向三大特性**:封裝、繼承、多型

- **封裝**: 封裝是將物件的資料（變數）和行為（方法）包裝在一起，並且隱藏內部細節，只對外暴露必要的接口。使用者不需要了解物件的內部實作細節，只需使用公開的方法來操作物件
```Java
public class Person {
    private String name;  // 私有變數，封裝內部狀態

    public String getName() {  // 公開方法，提供存取接口
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

- **繼承**: 繼承是子類別繼承父類別的屬性與方法，重用父類別程式碼，並能擴充或覆寫（Override）父類別功能
```Java
class Animal {
	public void move() {
		System.out.println( "move" );
	}
}
class Dog extends Animal {  //extends繼承父類別
    @Override
	public void move() {
		System.out.println( "run" );
	}
}

- **多載 (Overloading)**:在同一類別中，方法名稱相同，但參數型別、數量不同，稱為多載
```Java
class Hello {
    public void print() {
	    System.out.println( "" );
    }
    
    public void print(String hello) { //相同方法名稱print，但額外提供 String hello參數
	    System.out.println( hello );
    }
}
```

- **覆寫 (Overriding)**:子類別繼承父類別後，可以重新定義父類別的方法，稱為覆寫
```Java
class Animal {
	public void move() {
		System.out.println( "move" );
	}
}
class Dog extends Animal {
    @Override
	public void move() { //重新覆寫父類別方法
		System.out.println( "run" );
	}
}
```

- **多型 (Polymorphism)**:父類別變數可以指向子類別的物件，執行時根據物件型別呼叫對應方法，使得相同程式碼有不同行為，稱為多型
```Java
class Animal {
	public void move() {
		System.out.println( "move" );
	}
}
class Dog extends Animal {
	@Override
    public void move() {
		System.out.println( "run" );
	}
}
class Bird extends Animal {
    @Override
	public void move() {
		System.out.println( "fly" );
	}
}

Animal dog = new Dog();  //Animal變數，實際是 Dog物件
Animal bird = new Bird();//Animal變數，實際是 Bird物件
dog.move();  //會輸出 run
bird.move(); //會輸出 fly，輸出不同行為
```

- **單例模式(Singleton)**
```Java
class Singleton {
    private static Singleton instance;

    private Singleton(){}

    public static Singleton getInstance() {
        if ( instance == null ) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

- **氣泡排序法(Bubble Sort)** 每次從頭開始比較 2個元素，將較大(小)元素往後移動
```Java
public int[] bubbleSort(int[] arr) {
    int n = arr.length;
    boolean hasSwap = true;
    while ( hasSwap ) {
        hasSwap = false;
        for ( int i = 1; i < n; i++ ) {
            if ( arr[i-1] > arr[i] ) {
                swap(arr, i-1, i);
                hasSwap = true;
            }
        }
    }

    return arr;
}
```

- **插入排序法(Insertion Sort)** 每次由當前元素開始，往前尋找，直到找到適合插入的位置
```Java
public int[] insertionSort(int[] arr) {
    int n = arr.length;
    for ( int i = 0; i < n; i++ ) {
        int cur = arr[i];
        
        int j = i-1;
        while ( j >= 0 && cur < arr[j] ) {
            arr[j+1] = arr[j];
            j--;
        }

        arr[j+1] = cur;
    }

    return arr;
}
```

- **選擇排序法(Selection Sort)** 由當前元素開始，往後選擇最小(大)，並放到目前位置
```Java
public int[] selectionSort(int[] arr) {
    int n = arr.length;
    for ( int i = 0; i < n; i++ ) {
        int minIdx = i;
        for ( int j = i+1; j < n; j++ ) {
            if ( arr[j] < arr[minIdx] ) {
                minIdx = j;
            }
        }
        swap(arr, i, minIdx);
    }

    return arr;
}
```
