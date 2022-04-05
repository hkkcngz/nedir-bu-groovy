# Nedir Bu Groovy
Groovy Programlama Dili, Java platformu için geliştirilmiş çok yönlü bir dildir.
Android’de rastladığımız Build.Gradle, Groovy dili izle yazılmıştır ve syntax'ı bu yüzden farklıdır.

### Build Gradle Syntax Kuralları: 
- **Tanımlamalarda asla ; kullanılmaz,**
- **android { buildTypes { dev {} } == android.buildTypes.dev**
- **Sabit Tanımlama & Kullanımı**
  ```
    android {
        buildTypes {
            debug {
                buildConfigField "int", "FOO", "42"
                buildConfigField "String", "FOO_STRING", "\"foo\""
                buildConfigField "boolean", "LOG", "true"
            }
        }
    }
    ```

    **Usage:** BuildConfig.FOO

- **Resource Değiştirme**
    ```
    android {
        buildTypes {
            debug{
                resValue "string", "app_name", "My App Name Debug"
            }
            release {
                resValue "string", "app_name", "My App Name"
            }
        }
    }
    ```
    **Usage:** @string/app_name or R.string.app_name

- **Tanımlama ve String İçinde Kullanımı**
    ```
    def SERVER_HOST = "good-company.com"
    buildConfigField 'String', 'API_HOST', "\"${SERVER_HOST}\""
    ```
- **Tanımlamada def vs String…**

    Tanımlama yaparken **def** mi kullansak **String** mi kullansakta deklerasyonu öyle yapsak diyorsanız, buraya dikkat.
    ```
    def String name = "Guillaume" Yanlış Kullanım
    def name = "Guillaume" or String name = "Guillaume" Doğru Kullanım
    ```
    **def** genelde tipi belirsiz tanımlama yaparken kullanılır. Zaten String bir işleminiz varsa def e gerek yok. 

- **Println ve Parantezler** 
    **System.out.println("Hello");** yazmak isterseniz **println "Hello"** bu şekilde yazabilirsiniz.

    ```
    println "Hello"
    method a, b
    
    vs 
    
    println("Hello")
    method(a, b)
    ```
    Yine bazı parantezlerden kurtulabilirsin
    ```
    list.each( { println it } )
    list.each(){ println it }
    list.each  { println it } // Kısa kullanım
    ```
- **Equals ve ==**
   Karşılaştırma operatörü olarak == kullanılması daha iyi olacaktır. Çünkü == ayrıca null kontrolü de yapmaktadır.
    ```
    böyle kullanmak yerine 
    status != null && status.equals(ControlConstants.STATUS_COMPLETED)
    
    bunu kullanın
    status == ControlConstants.STATUS_COMPLETED
    ```
- **IF Else Null Kontrol**
    ```
    böyle kullanmak yerine  
    if (order != null) {
        if (order.getCustomer() != null) {
            if (order.getCustomer().getAddress() != null) {
                System.out.println(order.getCustomer().getAddress());
            }
        }
    }
    
    bunu kullanın
    println order?.customer?.address
    ```
- **Assert**
Dönen parametreleri, değerleri ve dahasını, assert ile kontrol edebilirsiniz.Java’daki assert’in aksine, "assert"lerin çalışması için etkinleştirilmesi gerekmez, bu nedenle "assert"ler her zaman kontrol edilir.
    ```
    def check(String name) {
        // name non-null and non-empty according to Groovy Truth
        assert name
        // safe navigation + Groovy Truth to check
        assert name?.size() > 3
    }
    ```
- **Elvis operator for default values**

    Bazen def result = name != null ? name : "Unknown" bu tarz tanımlamalar yazarız. Groovy dilinde bu tanımlamayı çok daha basit bir şekilde yandaki gibi yazabiliriz. def result = name ?: "Unknown" 

- **Try Catch**
    ```
    try {
        // ...
    } catch (Exception t) {
        // something bad happens
    }
    
    Exception kısmında 'any' or 'all' değerleri alır. 
    try {
        // ...
    } catch (any) {
        // something bad happens
    }
    ```


- **List**
    Liste oluşturmada yine Java'daki List kütüphanesini olduğu gibi kullanabiliyor. Tabi bunun daha yalın yine groovy hali de mevcut. Bu kısım opsiyonel çünkü Java'dan bildiğimiz listemiz yine sorunsuz çalışıyor. Detaylara ve daha fazla syntax bilgisini groovy'nin kendi dökümantasyonundan da ulaşabilirsiniz. (Bkz. http://groovy-lang.org/syntax.html#_lists). Şimdi Groovy'de liste tanımlamaya bakalım.

    ```
    def karisik = [1, "a", true]  
    
    
    def letters = ['a', 'b', 'c', 'd']
    
    yeni item eklemek için
    letters << 'e'   
    
    ---
    erişmek için
    assert letters[ 4] == 'e' 

    assert letters[-1] == 'e'

    letters[2] = 'C'             
    assert letters[2] == 'C'
    
    ```
