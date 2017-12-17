# Gem

Programlama dilleri belli kişiler veya topluluklar tarafından geliştirilir.
Ruby dili de internetteki herkesin katkı yapabileceği bir dil olsa da bu dili
yöneten, eklenen tüm özellikleri kontrol eden, açıklarını kapatan ve
güncellemelerini yayınlayan bir grup vardır.

Bu bahsettiğimiz , dili yöneten grup sınırlı sayıda kişiyden oluştuğu için
bu dilde olması beklenilen bütün özellikler bir anda yazılamıyor.

Dili kullananlar ise kendi ihtiyaçlarına uygun olarak dilin sağladığı
imkanlarla, dilin temel olarak sağlamadığı bazı özellikleri oluşturabiliyorlar.

Bu özelliklerin paketlenerek tüm Ruby geliştiricilerine sunulmuş haline de
**Gem** adı veriliyor.

Şu anda internette sayısı onbinlere yaklaşan sayıda faklı gem, geliştiricilerin
hizmetine sunulmuş durumda.

Geliştiriciler ihtiyaçlarını karşılayacak gemleri sistemlerine kurarak
kullandıkları Ruby diline kolaylıkla yeni özellikler eklemiş
oluyor.

## Gem Kurulumu

**Gemler** paketlenmiş halde internetteki gem sunucularında saklanan
dosyalardır.

Sisteminize bir gem kurmak ise oldukça basittir.

Örneğin `gem_adi` adındaki bir gemi yüklemek istiyorsak şu komutu terminale
yazmamız yeterli olacaktır:

```ruby
gem install gem_adi
```

Buradaki `gem_adi` yı yüklemek istediğiniz gemin paket adı ile değiştirirseniz
istediğiniz gem sisteminize kurulacaktır.

## Gem Kaldırma

Olur da sisteminizden bir gemi kaldırmak istiyorsanız da yazmanız gereken komut
aşağıdaki gibidir:

```ruby
gem uninstall gem_adi
```

## Gemlerin İncelenmesi

Gem kullanımına geçmeden önce bazı şeyleri kafamızda netleştirmeliyiz.

Öncelikle gemler de Ruby kodlarından oluşan dosyalardır.

Gemler dünya üzerindeki herkes tarafından geliştirilebildiği için genel bir
standart kullanıma sahip değildirler.

Bir gemi nasıl kullanacağımızı öğrenmek için çoğu zaman 
**dokümanlarını incelememiz gerekmektedir**.

Bu dokümanlar da çok büyük oranda **İngilizce** olmaktadır.

## Gem Kurulumu

Ruby dilinde ön tanımlı olarak kullanıma hazır halde olmayan, sadece sizin
kullanacağınızı belirttiğiniz taktirde aktifleştirilen modüller vardır.

Gemler de sisteme sonradan eklendiği için bu modüller gibi sadece biz onu
kullanacağımızı belirttiğimizde Ruby tarafından aktifleştirilir.

Örneğin terminalimizde renkli çıktılar vermemize olanak sağlayan `rainbow`
isimli gemi kullanmak istiyorsak;

Öncelikle `rainbow` gemini sistemimize yüklemeliyiz.

```ruby
gem install rainbow
```

Artık `rainbow` gemini sistemimizde kullanabiliriz.

Örneğin bir ruby kod dosyası oluşturalım ve çalıştırdığımızda
renkli çıktılar vermesini sağlayalım.

**Bunun için kodumuzun en başında `rainbow` gemimizi**
**kullanacağımızı belirtiriz.**

**Bunu da şu satırı kodumuzun başına ekleyerek yaparız:**

```ruby
require 'rainbow'
```



`rainbow` geminin dokümanlarını incelediğimizde şu kalıbı kullanarak
renkli yazılar yazabileceğimizi anlıyoruz.

```ruby
Rainbow(renklendirmek_istedigimiz_yazi).renk_adi
```

Örneğin ekrana **yeşil** renkte `Merhaba Dünya!` yazdırmak için

```ruby
puts Rainbow("Merhaba Dünya!").green
```

şeklinde kullanabiliriz.

Aynı şekilde yine dokümanları incelediğimizde bir yazının arkaplan rengini
değiştirmek için şu kalıbı kullanabildiğimizi anlıyoruz:

```ruby
Rainbow(arkaplani_renkli_yazi).bg(:renk)
```

Örneğin arkaplanı sarı şekilde `Merhaba Mars!` yazmak için

```ruby
puts Rainbow("Merhaba Mars!").bg(:yellow)
```

Parlak (daha kalın ve açık renkte) yazıdrmak için:

```ruby
Rainbow(parlak_yazilacak_yazi).bright
```

Altı çizili yazı yazmak için:

```ruby
Rainbow(alti_cizili_yazilacak_yazi).underline
```

kalıplarını kullanabilirsiniz.

Yukarıda bahsedilen biçimlendirmeleri birlikte de kullanabilirsiniz.

Örneğin `Merhaba` kelimesi kırmızı ve altı çizili, `Dünya!` kelimesi de mavi ve
arkaplanı sarı olacak şekilde bir yazı oluşturmak istersek

```ruby
Rainbow("Merhaba ").red.underline + Rainbow("Dünya!").blue.bg(:yellow)
```

Bu renkli yazıyı istersek bir değişkende de tutabiliriz.

```ruby
renkli_yazi = Rainbow("Merhaba ").red.underline + Rainbow("Dünya!").blue.bg(:yellow)
```

Ve bu değişkeni yazdırdığımızda da yine çıktı renkli olacaktır.

```ruby
puts renkli_yazi
```




-----

Bu doküman [Serhat Celil İLERİ](http://serhatcileri.com) ( GitHub: [@ileri](https://github.com/ileri) ) tarafından oluşturulmuştur.
