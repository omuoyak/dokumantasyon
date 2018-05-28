# Rails Cache ( Önbellekleme )

Cache teriminin Türkçe karşılığı için "önbellek" kelimesi kullanılabilir.
Cache kelimesinin okunuşu "keş" şeklinde telaffuz edilir. Aynı şekilde cache
işlemi yapmak anlamına gelen "**caching**" de ( okunuş olarak "**keşing**")
veya Türkçeye evrilmiş haliyle "**cacheleme**", ( okunuş olarak "**keşleme**" )
şeklinde de kullanılır.

Cache, bilgisayar dünyasında sıklıkla tekrarlanan veya daha önceden
kullanılacağı tahmin edilen verilerin, normal yollara göre daha hızlı
erişilebilecek bir şekilde depolanması olarak özetlenebilir.

**Rails** dünyasında da cache, **istek-cevap döngüsü** sırasında üretilen içeriklerin
depolanarak benzer istekler geldiğinde tekrar kullanılması şeklinde uygulanır.

Genel olarak bir uygulamayı hızlandırmanın veya performansını artırmanın en
etkili yollarından birisi olarak caching ön plana çıkar. Binlerce kullanıcıdan
gelen aynı istek tekrar tekrar işlenmek yerine ilk geldiğinde işlenerek sonucu
cachelenir ve diğer kullanıcılarınistekleri sistemde herhangi bir işleme
gerek kalmaksızın, sistem kaynaklarını da boşa yormadan cevaplanmış olur.

Rails bir takım cache özelliklerini içinde barındırır. Bu özellikleri
verimli şekilde kullanabilirseniz hem uygulamanızın cevap süresini kısaltabilir
hem de çok daha az kaynak kullanımına gerek duyacağınız için yüklü sunucu
giderleri yapmaktan kurtulabilirsiniz.

Bu dokümanda Rails Guide'ındaki bilgilerden yola çıkılarak basit olarak
Rails ve Cache kullanımı incelenecektir. Daha detaylı bilgiyi şu Rails Guide
sayfasında bulabilirsiniz:
[Caching with Rails: An Overview](http://guides.rubyonrails.org/caching_with_rails.html)

## Temel Önbellekleme ( Basic Caching )

Temel olarak Rails de üç tip caching vardır:
**Page Caching**, **Action Caching**, **Fragment Caching**

Öntanımlı olarak Rails fragment caching sağlar şekilde gelir.

**Page Caching** özelliğini aktifleştirmek için *actionpack-page_caching*,
**Action Caching**  özelliğini aktifleştirmek için de
*actionpack-action_caching* gemlerini **Gemfile** dosyanıza eklemeniz
gerekiyor.

Caching işlemi öntanımlı olarak sadece **production** yani yayınlanma ortamında
( production environment ) etkin olarak gelir. Diğer ortamlarda da denemeler
yapabilmek isterseniz `config/environments/` dizinindeki ayar dosyalarında
`config.action_controller.perform_caching` ayarını `true` yapmanız gerekiyor.

### Page (Sayfa) Caching

Sayfa caching mekanizması Web Sunucusundan gelen bir sayfa görüntüleme isteğini
, hiç Rails yapısının içlerine girmeden, daha önce oluşturulmuş olan halini
döndürerek yanıtlamak şeklindedir. Burada önemli olan nokta ise sayfanın durgun
( statik ) olmasıdır. Bu yöntemle üretilen sayfalar, Rails Guide'ında da
belirtildiği üzere "süper hızlı" da olsalar her durum için elveişli değildir.
Örneğin bir kimlik doğrulama veya benzeri bir istekten isteğe veya
kullanıcıdan kullanıcıya değişebilecek bir durum içeren sayfaların
tamamıyla cachelenerek herkese aynı sayfanın dönülmesi söz konusu olamaz.

### Action Caching

Page Cachingdeki belirttiğimiz kimlik doğrulama gibi işlemler devreye
girdiğinde ise sorunu Action Caching ile çözeriz. Action Caching de Page
Caching mekanizmasına benzer olarak çalışsa da gelen istekler öncelikle
Rails yapısına yönlendirilir, bu sayede cevap dönülmeden önce istenen
filtreler uygulanabilir.

Bu sayde önbelleğe alınan ( cachelenen ) sayfaların sonucu döndürülürken
kimlik doğrulaması veya başka bir kısıtlamaya da imkan sağlanır.

### Fragment Caching

Günlük hayatta kullandığımız ve oluşturduğumuz bir çok web uygulaması
Page Caching gibi çok kapsayıcı tek ve bütün bir cachelenmiş sayfa yerine
farklı önbellekleme ( caching ) özelliklerine sahip parçaların bir araya
gelmesiyle oluşur.

Bu gibi sayfanın farklı bölümlerinin farklı zaman
aralıklarında tekrar önbelleklenmesi ve zaman aşımına uğraması gereken
durumlarda Fragment Caching kullanılır. Zaten varsayılan olarak Rails'in
bir özelliği olarak gelen Fragment Caching en sıklıkla başvurulan önbellekleme
türü olarak gösterilebilir.

Rails'in kendi guide ındaki örnek üzerinden gidecek olursak, örneğin bir
sayfada ürünleri gösterecekseniz genellikle bu ürünlerin her seferinde
istek sunucuya geldiğinde işlenip veri tabanından ilgili bilgilerin
çekilmesine gerek yoktur. Aynı ürün aynı kişiye çokça defa da gösterilebilir
veya farklı kişilere de aynı ürün aynı şekilde gösterilebilir. Bu durumda
aynı isteğe her seferinde aynı sonuç döndürüleceği için önbellekleme burada
önemli bir verim artışı sağlayacaktır.

Örneğin yukarıda belirttiğimiz üzere bir sayfada ürünleri listeleyeceksek
önbelleklenmiş şekilde listelemek için şu tarz bir kod yazılacaktır:

```ruby
<% @products.each do |product| %>
  <% cache product do %>
    <%= render product %>
  <% end %>
<% end %>
```

Uygulamanız bu kodu içeren sayfaya ilk isteği aldığında Rails otomatik olarak
bu ürün için uniq ( benzersiz, eşsiz ) bir anahtarı da içeren yeni bir
önbellek girdisi oluşturacaktır. Örnek bir anahtar şöyle görünecektir:

```ruby
views/products/1-201505056193031061005000/bea67108094918eeba42cd4a6e786901
```

Bu anahtar aslında içinde birçok bilgiyi de barındırmaktadır. Detaylarını
yukarıda adresini verdiğim Rails Guide sayfasından okuyabileceğiniz bu bilgiler
sayesinde Rails kullandığı bilginin tabiri caizse "bayatlamış" olup olmadığını
anlayarak gerektiğinde "tazesini" oluşturmakta kullanır. Bu işleme de
`key-based expiration` adı verilir. Sanırım `anahtar bazlı sonlandırma` olarak
Türkçeye çevrilebilir.

Fragment Cache ile önbelleklenen veriler görünüm dosyasında (view dosyası,
`.html.erb`) bir değişim olduğunda da yeniden oluşturularak yeni haliyle
önbelleklenir.

Herhangi bir nedenle bir fragment cache'in yenisi oluşturulursa eskisi de
otomatik olarak sistemden silinir.

Rails aynı zamanda fragment caching yaparken belirli koşullara bağlı olarak
önbellekleme yapıp yapmamanıza da olanak sağlar. Bunu için `cache_if` ve
`cache_unless` kullanılır.

Örneğin ürünlerin sadece admin olmayan kullanıcılar için önbelleklenmesi için
şöyle bir kod yazılabilir:

```ruby
<% cache_unless admin?, product do %>
  <%= render product %>
<% end %>
```

#### Collection Caching

Koleksiyon önbellekleme (collection caching) de Fragment Caching başlığı
altında incelenir.

Bir render işlemi yaparken koleksiyon için oluşturulan çıktılar da aynı
şekilde önbelleklenebilirler.Yukarıda ürünleri teker teker döngüde
önbellekleyeceğimizi belirtirken burada tek seferde tüm koleksiyon
için render edilen çıktıyı önbellekleyebiliyoruz.

Koleksiyon önbelleklemeyi aktifleştirmek için tek yapmanız gereken ise
render fonksiyonuna parametre olarak `cached: true` eklemek.

Koleksiyon önbelleklemesi sayesinde daha önce önbelleğe alınmış şablonlar
çok daha hızlı şekilde çekilebilmektedir.

### Russian Doll ( Rus Bebeği , Matruşka ) Caching

İsminden de anlaşılacağı üzere bu önbellekleme tipi iç içe geçmiş yapılar
üzerinde önbellekleme işlemi yapmamızı sağlar. Buradaki önemli nokta ise
bu iç içe geçen yapıların her biri için önbellekleme yapılıyor olabilir.

Bu tip önbelleklemenin katkısı ise örneğin yukarıdaki örnekte verdiğimiz bir
ürün güncellendiğinde diğer tüm önbelleklenen parçalar aynen kullanılabilir.
Sadece güncellenen parça yeniden önbelleklenir. Bunu sağlayan da yukarıda
kısaca bahsettiğimiz `key-based expiration` özelliğidir.

Örnek verecek olursak, şunun gibi bir görünüm dosyamız varsa:

```ruby
<% cache product do %>
  <%= render product.games %>
<% end %>
```

ve oradan da bu kod çalıştırılıyorsa:

```ruby
<% cache game do %>
  <%= render game %>
<% end %>
```

`game` in herhangi bir özelliği değiştiğinde yeni bir anahtar ( `key` )
oluşturulacağı için yeniden önbelleklemeye tabii tutulacaktır.
Ancak `game` i içeren `product` nesnesinde herhangi bir değişiklik olmadığı
için anahtarı aynı kalacaktır. Bu yüzden tekrardan önbelleklemeye tabii
tutulmayacaktır. Bu da `product` için oluşturulan eski bir görünümün
kullanıcıya sunulmasına neden olacaktır. Bunu engellemek için `product` a
ait olan `game` nesnesinin model dosyasında `product` ile olan ilişkisini
belirtirken `touch: true` parametresini de ekleriz.

Örneğin yukarıdaki yapı için `Product` ve `Game` modelleri şöyle olacaktır:

```ruby
class Product < ApplicationRecord
  has_many :games
end

class Game < ApplicationRecord
  belongs_to :product, touch: true
end
```

`touch` özelliğini `true` yaptığımızda `Game` den üretilen bir nesne
güncellendiğinde ilişkili olduğu `Product` tan türetilen nesnenin de
son güncellenme tarihi değiştirilecektir. Bu sayede bu `product` nesnesi
için oluşturulan önbellek dosyası yenisi ile değiştirilecektir.

### Shared Partial Caching

Shared partial dosyaları da önbelleklenebilir. Bunu için de tek yapmanız
gereken render fonksiyonuna `cached: ture` parametresini eklemektir.

```ruby
render(partial: 'hotels/hotel', collection: @hotels, cached: true)
```

### Bağımlılıkların Yönetimi ( Managing Dependencies )

Önbelleği doğru şekilde geçersiz kılmak için, önbelleğe alma bağımlılıklarını
doğru bir şekilde tanımlamanız gerekir.

Aslında genellikle hiç böyle bir önlem almanıza gerek kalmadan Rails sizin için
çoğu durumda bu işi halleder. Yine de bazen bu bağımlılıkları özel olarak
tanımlamanız gerekebilir.

Bu konu `Implicit Dependencies` ,`Explicit Dependencies` ve
`External Dependencies` olarak üçe ayrılıyor.

Çok nadir durumlarda gereken işlemler olduğu için bu
yazıda detaylarına girmeye gerek olmadığını düşünüyorum. İncelemek isterseniz:
[1.6 Managing dependencies](http://guides.rubyonrails.org/caching_with_rails.html#managing-dependencies)

### Low-Level ( Düşük Seviyeli, Alt Düzey ) Caching

**Page Caching**, **Fragment Caching** gibi önbellekleme türlerinde hep
view dosyalarını yani kullanıcıya döndürülen görüntüleri önbellekleriz.
Ancak Rails de önbellekleme bununla sınırlı değildir. İstediğiniz herhangi
bir tipteki veriyi de Rails'in önbellekleme mekanizmasıyla önbellekleyerek
uygulamanızda performans artışı sağlayabilirsiniz.

Low-level caching in en etkili uygulaması `Rails.cache.fetch` metodunu
kullanmaktır. Bu metod önbelleğe yazma ve okuma işlemlerinin ikisini de yapar.

Tek bir argumanla çalıştırıldığında, ilgili arguman için oluşturulan anahtar
elde edilir ve önbellekten ilgili anahtara ait veri çekilir.

Eğer ki bir blok verilmişse de bir `cache miss` durumunda ( önbelleğin geçersiz
veya eski olması, veya hiç bulunmaması ) blok çalıştırılarak sonuç, verilen
anahtar ile önbelleğe kaydedilir ve dönüş değeri döndürülür. `cache hit` yani
başarılı şekikde önbellekten veri çekilmesi durumunda ise blok hiç
çalıştırılmadan önbelleklenen değer döndürülür.

Örnek olarak şu şekillerde kullanılabilir:

```ruby
class Product < ApplicationRecord
  # Tek argümanlı kullanım
  def products_count
    Rails.cache.fetch('count_of_products', Product.count)
  end

  # Blok ile kullanım
  def competing_price
    Rails.cache.fetch("#{cache_key}/competing_price", expires_in: 12.hours) do
      Competitor::API.find_price(id) # Müşteri API'ından çekilen fiyat
    end
  end
```

Örnekte görüldüğü gibi `expires_in:` parametresiyle önbelleklenmiş verinin
ömrünü ( Time to Live - TTL ) belirleyebiliyoruz.

### SQL Caching

Aslında burada pek anlatılacak bir şey yok. Her şeyi Rails hallediyor.

Veritabanından bir bilgi çektiğinizde otomatik olarak sonucu önbelleklenir.
Tekrar aynı veriyi çekmek istediğinizde veritabanına gidilerek SQL sorgusu
çalıştırılmak yerine önbelleklenmiş sonuç dönüdürülür.

```ruby
class ProductsController < ApplicationController

  def index
    # Tüm ürünleri çeken sorguyu çalıştır
    @products = Product.all

    ... # Başka bir şeyler yap...

    # Aynı sorguyu tekrar çalıştır
    @products = Product.all
  end

end
```

-----

Yukarıdaki kodda iki defa veritabanında SQL sorgusu çalıştırılıyormuş
gibi görünse de aslında sadece ilk seferinde çalıştırılır. İkinci defa aynı
sorgu çalıştırıldığında önceki seferde önbelleklenen sonuç döndürülür. Bu
şekilde veritabanı işlemleri de optimize edilmiş olur.

Genel olarak değineceğimiz konular bu kadardı. Yukarıda da dediğim gibi,
bu doküman Rail Guide'ından yararlanılarak oluşturulmuştur. Daha detaylı
şekilde incelemek ve daha derin konuları öğrenmek istiyorsanız:
[Caching with Rails: An Overview](http://guides.rubyonrails.org/caching_with_rails.html)

-----

Bu doküman [Serhat Celil İLERİ](http://serhatcileri.com) ( GitHub: [@ileri](https://github.com/ileri) ) tarafından oluşturulmuştur.
