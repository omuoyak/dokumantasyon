# Ruby'de Dosya İşlemleri

## Genel Kurallar

- Bir dosyayı açtıysak, kapatmamız lazım.
- Elle kapatmak yerine dosya işlemlerini bir blok içinde yapın.
- Dosya açma modunu gerektiği kadar verin.


### Dosya Açma Modu Nedir?

Dosya açma modu, yazdığınız programın bir dosyayı açarken hangi amaçlarla
kullanacağını belirttiği bir stringtir.

Bu modlar şunlardır:

| Dosya Açma Modu | Kısa Açıklama            | Uzun Açıklama                                                                                                                                                                                                  |
|-----------------|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `"r"`           | **Sadece Oku**           | Dosyayı sadece okunabilir olarak aç. Dosyanın başından itibaren sadece okuyabilecek (yazma işlemi yapamayacak) şekilde açmanızı sağlar.Herhangi bir mod belirtmezseniz dosya bu modda açılacaktır (varsayılan) |
| `"r+"`          | **Oku ve Yaz**           | Dosyayı hem okunabilir ve yazılabilir olacakşekilde açar.                                                                                                                                                      |
| `"w"`           | **Sadece Yaz**           | Dosyaya sadece yazma işlemi yapılabilir, içinden bilgi okunamaz. İçindeki mevcut bilgiler **SİLİNİR!**                                                                                                         |
| `"w+"`          | **Yaz ve Oku**           | Dosyaya okuma ve yazma işlemi yapılır. Belirtilen dosya yoksa oluşturulur, varsa içindekiler **SİLİNİR** ve öyle düzenlenir.                                                                                   |
| `"a"`           | **Sonuna Yaz**           | Dosyanın sonuna ekleme yapılır. Belirtilen dosyayoksa oluşturulur, varsa,içindeki bilgiler korunarak en sona eklemeler yapılır. Okuma yapılamaz.                                                               |
| `"a+"`          | **Sondan Oku ve Yaz**    | Dosyanın sonundan itibaren okuma ve yazma işlemleri yapılır. Dosya yoksa oluşturulur, varsa sondan işlemler yapılmaya devam edilir.                                                                            |
| `"b"`           | **Derlenmiş Dosya Modu** | Derlenmiş yani işlemcinin anlayabileceğiikilik tabandaki komutlar haline çevrilmiş dosyalar için kullanılır.                                                                                                   |
| `"t"`           | **Metin Dosyası Modu**   | Sadece metin içeren dosyalar için kullanılır.                                                                                                                                                                  |


## Dosya Oluşturma

Ruby'de bulunduğunuz dizinde veya belirttiğiniz yoldaki bir dizinde yeni bir
dosya oluşturmak için `File.new` fonksiyonu kullanılır.

Bu fonksiyonu çağırırken 2 tane parametre verilir. 

Bunlar dosya adı ve dosyayı açma modudur.

Örnek kullanım:

```ruby
File.new("dosya_adi", "w+") 
# w+ modunda dosya yoksa oluşturulur ve dosyada okuma ve yazma işlemleri yapılabilir.
```

Yukarıdaki kodda sadece dosya oluşturuldu, ancak oluşturduğumuz dosyayı kullanamıyoruz.

Ayrıca dosyayı oluşturduk ancak kapatmadık, bu yüzden programımız çalıştığı
sürece dosya da program tarafından kullanılıyor, yani bir diğer tabirle
işgal ediliyor olacaktır.


Bunun önüne geçmek için 3 temel farklı yol izleyebiliriz.

Aşağıda 3 faklı senaryo için 3 farklı kullanım mevcuttur.

Ancak önce açtığımız bir dosyayı nasıl kapatacağımızı öğrenelim.



### Dosya Kapatma

Ruby veya herhangi başka bir programlama dilinde bir program yazarken dosya
işlemleri sırasında bir dosyayı kullanmak üzere açtıysanız işiniz bittiğinde
o dosyayı kapatmanız gerekmektedir.

Eğer ki dosyayı açıp da kapatmadan bırakırsanız, programınız çalıştığı süre
boyunca hard diskinizden (veya ilgili depolama aracından) dosya kullanılmaya
devam edecektir. Bu da dosyayı başka programların açamamasına , açsa da 
üzerinde değişiklğik yapamamasına, harici bir çıkarılabilir diskte olması
durumunda ise belleği güzenli çıkar işlemiyle çıkarmak istediğinizde 
kullanıcının "Bu disk kullanılıyor" tarzında hata almasına, eski işletim
sistemine sahip cihazlarda bu işlem yapılırken çıkartılan çıkarılabilir
diskin zarar görmesine kadar bir çok istenmeyen duruma neden olabilir.

Bu yüzden bir dosyayı açıyorsak mutlaka işimiz bittiğinde kapatmalıyız.

Kapatma işlemi `.close` fonksiyonu ile yapılmaktadır.

Örnek kullanım
```ruby
# Dosya açılmış ve "dosya" isimli değişkende tutulmakta olduğu varsayımıyla 

dosya.close
```


### Senaryo 1 - Dosyayı oluştur, işi bitir

Bu senaryoda sadece bilgisayarımızda bir dosya oluşturup, içine herhangi bir
şey okuyup yazmayacağımızı varsayalım.

O halde dosyayı oluşturup, ruby programımızda kapatılmasını sağlayalım.

Burada herhangi bir ekstra değişken kullanmamak için Ruby'nin zincileme
fonksiyon çağırma özelliğinden faydalanacağız.

Yapacağımız işlemler:
- Dosyayı oluştur
- Hiç bir şey yapmadan hemen kapat

Bu işleri yapan kodumuz şöyle olacaktır:

```ruby
File.new("yeni_dosya.txt", "w").close 
```

Yukarıdaki kodda `yeni_dosya.txt` adında bir dosya `w` moduyla açılmak üzere 
oluşturulur. Hemen ardından ise oluşturulan dosyamız hiç bir işlem yapılmadan
kapatılır. Böylelikle ekstradan bir değişken de kullanmaya gerek kalmaz.



### Senaryo 2 - Dosya oluştur değişkene ata, sonda kapat

Bu senaryoda ise bilgisayarımızda bir dosya oluşturup, bu oluşturduğumuz
dosyayı Ruby programımızda bir değişkende tutup, yine herhangi bir işlem
yapmadan kapatma senaryosunu inceleyeceğiz.

Dosyamızı oluşturduktan sonra arada başka işlemler yapacaksak bu senaryoda
anlatılan yöntemi kullanabiliriz ancak genelde 3. senaryoyu kullanmak
daha uygun olacaktır.

Yapacağımız işlemler:
- Dosyayı oluştur
- Oluşturduğun dosyayı bir değişkende tut
- Arada başka işlemler yapacaksan yap
- Dosyayı kapat

```ruby
dosya = File.new("yeni_dosya.txt", "w")
puts 'Dosya oluşturuldu ve hala açık'
dosya.close
puts 'Dosya kapatıldı'
```


### Senaryo 3 - Dosyayı oluştur, blok olarak kullan

Bu senaryoda ise dosyamızı oluştururken blok kullanımı yöntemine başvuracağız.

Bu yöntemde açtığınız dosyayı sonradan tekrardan elle kapatmanız gerekmiyor.

Eğer geçerli bir nedeniniz yoksa dosya işlemlerinde blok yöntemini kullanın!

Ancak `File.new` fonksiyonu blok kullanımını desteklememektedir.

Bu yüzden dosya oluşturmaya yarayan `File.new` yerine , asıl amacı dosya
okumak olan ancak dosya oluşturmada da çokça kullanılan `File.open`
fonksiyonunu kullanacağız.

```ruby
File.open("yeni_dosya.txt", "w") do |dosya|
  puts 'Dosyanız oluşturuldu ve hala açık'
end
puts 'Dosyanız otomatik olarak kapatıldı'
```

Bu yöntemde diğerlerinden farklı olarak `File.new` yerine `File.open`
kullandık. Genel olarak pratik kullanımda `File.new` den ziyade `File.open`
çok daha sıkça kullanılır.



## Dosya Okuma

Dosya işlemlerinde sıklıkla kullanılan işlemlerden birisi de dosya okumadır.

Varolan bir dosyanın içindekileri okumak için temel olarak seçebileceğimiz
2 farklı yol vardır.

Bu yollara değinmeden önce, küçük bir bilgilendirme yapalım.

Dosya okuma işlemlerini `File.read` veya `File.open` ile yapabiliriz.
Ancak burada amacımızı doğrudan açğıa vuran fonksiyon `File.read` olduğu
için sadece dosya okuma işlemi yapacaksak `File.open` yerine 
**`File.read`** fonksiyonunu tercih edelim. Hem bu şekilde dosya açma 
modunu belirtmemize gerek de kalmayacaktır.

```ruby
File.read("dosya_adi.txt")
```

Evet, az önce dosya okumanın 2 yolu olduğundan bahsetmiştik.Bunlar:

**1 - Dosyanın tamamını okuyup bir stringe atama**

**2 - Dosyayı satır satır okuyup, her satırı diziye bir eleman olarak ekleme**

Bu yöntemlerden hangisini kullanacağınız kullanım senaryonuza bağlıdır.

Örneğin bir dosyanın tümünde bir arama gerçekleştirecekseniz toplu halde
okumak işinizi kolaylaştıracaktır.

Ancak bir kelimenin kaç satırda geçtiğini sayacaksanız da satır satır okumak
işinizi çok daha kolaylaştıracaktır.

Gelin bu 2 yolu inceleyelim


### Dosyanın Tek Parça Olarak Okumak

Bu yöntemde dosyanın içindeki tüm bilgiler bir string olarak alınacaktır.

Alt satıra geçme elemanı olan `\n` karakteri de stringin içinde yer alacaktır.

Örneğin `isimler.txt` dosyamızın içeriği aşağıdaki gibi olsun.

```
durmuş hoca
serhat
taha
ömer
tunahan
```

Bu dosyayı okumak için `File.read` fonksiyonunu aşağıdaki gibi kullanırız.

```ruby
File.read("isimler.txt")
```

Bu komutun bize vereceği string ise şu şekildedir:

```ruby
"durmuş hoca\nserhat\ntaha\nömer\ntunahan\n"
```

Okuduğumuz dosyanın içeriğini bir değişkene atayabiliriz.

```ruby
dosya_icerigi = File.read("isimler.txt")
```

Artık bu değişkeni bir string olarak kullanabiliriz.
Örneğin dosyanın içindeki tüm karakterlerin büyüğe çevrilmiş halini
elde edebiliriz.

```ruby
dosya_icerigi = File.read("isimler.txt")
dosya_icerigi.upcase
```

*Türkçe karakterler dönüşüme uğramayacaktır.*

```ruby
"DURMUş HOCA\nSERHAT\nTAHA\nöMER\nTUNAHAN\n"
```

Gördüğünüz gibi satırlar `\n` karakteriyle ayrılıyor.
Satır satır kullanmak için bu okuma yöntemi elverişli değil.



### Dosyayı Satır Satır Okumak

Çoğu zaman bir dosyanın tüm içeriğiyle değil, aynı anda sadece bir
satırıyla ilgilenmek, bir satırı üerinde işlem yapmak isteriz.

Bu durumlar için ise `File.readlines` fonksiyonunu kullanırız.

Örneğin yukarıdaki örnekteki `isimler.txt` dosyasını bu sefer de
`File.readlines` ile okursak 

```ruby
File.readlines("isimler.txt")
```

Bu işlem bize şu diziyi dönecektir:

```ruby
["durmuş hoca\n", "serhat\n", "taha\n", "ömer\n", "tunahan\n"]
```

Gördüğünüz üzere bu fonksiyonun bize döndürdüğü değer bir String değil,
Stringlerden oluşan bir dizi, yani string dizisidir.

Biz bu diziyi bir değişkene atayarak yada doğrudan döndüğü değer üzerinden
bir döngü oluşturarak satırlar üzerinde gezinebiliriz.



#### Bir değişkene atayarak dolaşma örneği

```ruby
satirlar = File.readlines("isimler.txt")
satirlar.each do |satir|
  puts satir
end
```

Ekran çıktısı aşağıdaki gibi olacaktır:

```ruby
durmuş hoca
serhat
taha
ömer
tunahan
```

Veya satır satır okuduğumuzu daha iyi anlamamıza yaracak bir örnek olarak
her satırın uzunluğunu yazdıracak olursak:

```ruby
satirlar = File.readlines("isimler.txt")
satirlar.each_with_index do |satir, i|
  puts "#{i+1}. satırın uzunluğu: #{satir.length}"
end
```

Çıktısı aşağıdaki gibi olacaktır:

```ruby
1. satırın uzunluğu: 12
2. satırın uzunluğu: 7
3. satırın uzunluğu: 5
4. satırın uzunluğu: 5
5. satırın uzunluğu: 8
```

Gördüğünüz gibi satır satır işlem yapmada işimizi oldukça kolaylaştırıyor.



#### Değişkene atamadan doğrudan değeri kullanma örneği

Yukarıdakiyle aynı işlemleri yapan kodları arada herhangi bir ek değişken
kullanmadan aşağıdaki gibi de yapabiliriz:

```ruby
File.readlines("isimler.txt").each do |satir|
  puts satir
end
```

```ruby
durmuş hoca
serhat
taha
ömer
tunahan
```

Diğer örnek ise şu şekilde:

```ruby
File.readlines("isimler.txt").each_with_index do |satir, i|
  puts "#{i+1}. satırın uzunluğu: #{satir.length}"
end
```

Dosya okuma işlemlerini yaparken `File.read` ve `File.readlines` yerine
önce dosya açılıp daha sonra okuma işlemi de yapılabilir.

Örneğin dosyayı tümüyle okuyacaksak `File.read` yerine 
`File.open("dosya_adi").read` kullanabiliriz.

Yine ayın şekilde, satır satır okuma yapacaksak `File.readlines`
yerine `File.open("dosya_adi").readlines` fonksiyon zincirini kullanabiliriz.



## Dosyaya Yazma

Dosyaya yazmanında mantığı yine dosyayı açmaktan geçer.

Bu yüzden öncelikle dosyamızı **yazılabilir** modda açıp, ardından üzerinde
istediğimiz işlemleri gerçekleştireceğiz.

Yazma işlemleri için genellikle dosya açma işlemini blok kullanarak yaparız.

Ancak burada dikkat etmemiz gereken nokta, 
**dosya açma modunun yazmaya izin vermesi gereklidir.**

Herhangi bir okuma modu belirtmezsek sadece okunabilir modda açacağı için
`File.open` metoduna 2. parametre olarak `"w"` , `"w+"` `"a"` yada `"a+"` 
açma modlarından birini vermeliyiz.

Dosyanın içine yazmak için `.write` , `.puts` , `.print`
fonksiyonlarını kullanabiliriz.

Burada dikkat etmemiz gereken ise, `write` ve `print` fonksiyonları stringin
sonuna satır sonu karakteri ( `\n` ) eklemediği için dosyada da
yeni satıra geçilmeyecektir.

Eğer eklediğimiz metinden sonra yeni satıra geçmek istiyorsak ya 
`write` veya `print` e verdiğimiz stringin sonuna `\n` karakterini
eklemeliyiz yada kısa yoldan `puts` kullanarak satır sonu karakterinin
otomatik olarak eklenmesini sağlayabiliriz.

```ruby
File.open("isimler.txt", "a+") do |dosya|
  dosya.write "celil"
end
```

Dosyanın içeriğini okuyacak olursak

```ruby
File.read("isimler.txt")
```

```ruby
"durmuş hoca\nserhat\ntaha\nömer\ntunahan\ncelil"
```

Gördüğünüz gibi okunan stringin sonunda `\n` karakteri bulunmamaktadır.

Bu dosyanın üzerine yazmaya devam edersek

```ruby
File.open("isimler.txt", "a+") do |dosya|
  dosya.puts "ileri"
end
```

dosyanın içeriğini okumak için

```ruby
File.read("isimler.txt")
```

yaptığımızda aşağıdaki stringi verecektir

```ruby
"durmuş hoca\nserhat\ntaha\nömer\ntunahan\ncelilileri\n"
```

Gördüğünüz gibi `celil` in sonunda `\n` karakteri yok ancak son eklenen
`ileri` nin sonunda `\n` karakteri var.

Bu stringi puts ile ekrana bastıracak olursak çıktısı aşağıdaki gibi olacaktır:

```ruby
durmuş hoca
serhat
taha
ömer
tunahan
celilileri
```



## Diğer Dosya Fonksiyonları

### Dosyanın Gerçek Yolunu Öğrenme
`File.absolute_path` fonksiyonuyla bir dosyanın gerçek dosya yolunu
 öğrenebilirsiniz.

 Örnek kullanım:

 ```ruby
File.absolute_path("isimler.txt")
# "/home/sci/isimler.txt"
 ```

-----

### Dosyaya Son Erişilen Tarihi Öğrenme
`File.atime` fonksiyonuyla bir dosyaya en son erişilen zamanı öğrenebilirsiniz.

Örnek kullanım:

```ruby
File.atime("isimler.txt")
# 2017-12-04 20:31:38 +0300
```

-----

### Dosyanın baz adını öğrenme
`File.basename` fonksiyonuyla bir dosyanın baz adını öğrenebilirsiniz.

```ruby
File.basename("/home/sci/isimler.txt")
# "isimler.txt"
```

Eğer ki bir dosyanın uzantısını biliyor ve uzantısız baz adını öğrenmek 
istiyorsanız `File.basename(dosya_yolu, uzantı)` olarak kullanabilirsiniz.

```ruby
File.basename("/home/sci/isimler.txt", ".txt")
# "isimler"
```

Okuduğunuz dosyanın uzantısını bilmiyor veya uzantısının ne olduğu sizin
için fark etmiyor, sadece uzantısız baz adını öğrenmek istiyorsanız

```ruby
File.basename("/home/sci/isimler.txt", ".*")
# "isimler"
```

Burada `".*"` ın anlamı: `.` ve sonrasında ne gelirse olarak özetlenebilir.

-----

### Dosya Sil

`File.delete` fonksiyonuyla istediğimiz (silme yetkimiz olan) bir dosyayı silebiliriz.

```ruby
File.delete("isimler1.txt")
```

-----

### Dizin mi? kontrolü

Verdiğiniz dosya yolundaki dosyanın biz dizin olup olmadığını öğrenmek
için `File.directory?` fonksiyonunu kullanabilirsiniz.

Örneğin bulunduğumuz dizinde `Downloads` adında bir dizin ve
`isimler.txt` adında bir dosya bulunmaktadır.

```ruby
File.directory?("Downloads")
# true
File.directory?("isimler.txt")
# false
```

-----

### Dosyanın bulunduğu dizin yolu
`File.dirname` fonksiyonuyla yolunu verdiğimiz dosyanın bulunduğu dizinin
yolunu verir. (tam yolu değil, verdiğimiz yoluna göre bulunduğu dizin)

```ruby
File.dirname("/home/sci/isimler.txt")
# "/home/sci"
```

-----

### Dosya var mı?
Bir dosya yolu vererek, orada gerçekten bir dosya olup olmadığını anlamak
için `File.exist?` fonksiyonunu kullanırız.

Örneğin bir dosya varsa farklı, yoksa farklı davranacak bir program yazmak
istiyorsanız bu fonksiyon işinize yarayacaktır.

```ruby
File.exist?("tunahan.cs")
# false
File.exist?("isimler.txt")
# true
```

-----

### Dosya Uzantısı Öğrenme
`File.extname` fonksiyonunu kullanarak yolunu verdiğimiz bir dosyanın
uzantısını elde edebiliriz.

```ruby
File.extname("isimler.txt")
# ".txt"
```

-----

### Dosya tipi öğrenme
Bu bilgi biraz erken bir bilgi olacak ancak, bir dosya `dosya` , `dizin`, `link` gibi farklı tiplerde olabilir. 

Bir dosyanın hangi tipten olduğunu öğrenmek için `File.ftype` fonksiyonunu
kullanabiliriz.

Sonucunda `"file”, “directory”, “characterSpecial”, “blockSpecial”, “fifo”, “link”, “socket”, “unknown”` değerlerinden birini dönecektir.

```ruby
File.ftype("isimler.txt")
# "file"
```

-----

### Dosya yolu oluşturma
Farklı işletim sistemlerinde dosya yolları farklı karakterlerle birleştirildiği
için eğer ki yazdığımız programın her işletim sisteminde sorunsuz çalışmasını
istiyorsak bir dosya yolu oluştururken elle oluşturmak yerine
`File.join` fonksiyonunu kullanmalıyız.

Bu fonksiyon kendine verdiğimiz parametreleri kullandığımız işletim sistemine
uygun olarak birleştirerek bize bir dosya yolu verektir.

Örneğin Unix mimarisini kullanan bir işletim sisteminde aşağıdaki kod
ve çıktısı şöyledir:

```ruby
File.join("home", "sci", "isimler.txt")
# "home/sci/isimler.txt"
```

Aynı kodu bir Windows işletim sisteminde çalıştırsak sonuç şu şekilde olacaktı:

```ruby
File.join("home", "sci", "isimler.txt")
# "home\sci\isimler.txt"
```

------

### Dosyanın Son Değiştirilme Tarihi Öğrenme
`File.mtime` fonksiyonuyla bir dosyanın en son ne zaman değiştirildiğini öğrenebilirsiniz.

Örnek kullanım:

```ruby
File.mtime("isimler.txt")
# 2017-12-04 20:31:35 +0300
```

------

### Dosya yeniden adlandırma
`File.rename` fonksiyonunu kullanarak bir dosyayı yeniden adlandırabiliriz.

Bu fonksiyona ilk parametre olarak şu anki dosya adını, ikinci parametre
olarak da yeni dosya adını verdiğimizde dosyayı yeniden isimlendirecektir.

```ruby
File.rename("isimler.txt", "derse_girenler.txt")
```

-----

### Dosya boyutu öğrenme
`File.size?` fonksiyonuyla bir dosyanın boyutunu öğrenebiliriz.

Olmayan bir dosya için `nil` değerini döndürecektir.

```ruby
File.size?("isimler.txt")
# 50
File.size?("olmayan_dosya")
# nil
```

**Not**: `.size` diye bir fonksiyon da vardır ve aynı işlevi görmektedir
ancak eğer dosya yoksa hata vermektedir. Hata almak yerine `nil` 
sonucunun dönmesini istiyorsak `.size?` fonksiyonunu kullanacağız.

-----

### Dosya boş mu?
Bir dosyanın var olduğu halde içinin boş olup olmadığını öğrenmek için 
`File.zero?` fonksiyonunu kullanırız.

Dosya yoksa veya var olup da içi doluya `false` dönecektir.

```ruby
File.zero?("ici_bos_dosya")
# true
File.zero?("isimler.txt")
# false
File.zero?("olmayan_dosya")
# false
```

-----

### Daha Fazla?

Bu dokümanda anlatılanlar temel ve en çok kullanılan dosya işlemleri idi.

Eğer tüm dosya fonksiyonlarına ulaşmak istiyorsanız [https://ruby-doc.org/core-2.2.0/File.html](https://ruby-doc.org/core-2.2.0/File.html)
 adresinde bulabilirsiniz.



-----

Bu doküman [Serhat Celil İLERİ](http://serhatcileri.com) ( GitHub: [@ileri](https://github.com/ileri) ) tarafından oluşturulmuştur.
