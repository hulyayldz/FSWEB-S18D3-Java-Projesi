# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın.

1.  Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT o.ograd, o.ogrsoyad ,i.atarih FROM ogrenci as o, islem as i WHERE o.ogrno=i.ogrno ORDER BY o.ograd,o.ogrsoyad asc;

2.  Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    SELECT k.kitapadi, t.turadi FROM kitap as k, tur as t
    WHERE k.turno = t.turno
    AND t.turadi IN ('Fıkra', 'Hikaye');

3.  10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif
    FROM ogrenci as o, kitap as k, islem as i
    WHERE o.ogrno=i.ogrno
    AND i.kitapno = k.kitapno
    AND o.sinif in ('10B','10C')
    order by o.ograd;

    #join ile yazın

4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

   SELECT o.ograd, o.ogrsoyad ,i.atarih FROM ogrenci as o
   INNER JOIN islem as i
   ON o.ogrno=i.ogrno
   ORDER BY o.ograd,o.ogrsoyad asc;

5.  Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    SELECT k.kitapadi, t.turadi FROM kitap as k INNER JOIN tur as t
    ON k.turno = t.turno
    WHERE t.turadi IN ('Fıkra', 'Hikaye');

6.  10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif FROM ogrenci as o
    INNER JOIN islem as i ON o.ogrno=i.ogrno
    INNER JOIN kitap as k ON i.kitapno = k.kitapno
    WHERE o.sinif in ('10B','10C')
    order by o.ograd;

7.  Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    SELECT o.ograd,o.ogrsoyad,i.atarih
    FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno = i.ogrno;

8.  Kitap almayan öğrencileri listeleyin.
    SELECT o.ograd,o.ogrsoyad,i.atarih
    FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno = i.ogrno
    WHERE i.ogrno IS NULL;

9.  Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    SELECT k.kitapno,k.kitapadi, count(i.kitapno) AS 'ALIM SAYISI'FROM kitap k
    INNER JOIN islem i
    ON k.kitapno = i.kitapno
    GROUP BY k.kitapno,k.kitapadi
    ORDER BY k.kitapno asc;

10. Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT k.kitapno,k.kitapadi,count(i.kitapno) as total
    FROM kitap as k left join islem as i on i.kitapno = k.kitapno
    group by k.kitapno,k.kitapadi
    ORDER BY k.kitapno asc;

11. Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
    SELECT o.ograd,o.ogrsoyad,k.kitapadi
    FROM ogrenci as o inner join islem as i on i.ogrno = o.ogrno
    INNER JOIN kitap as k on i.kitapno = k.kitapno;

12. Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    SELECT o.ograd,o.ogrsoyad,k.kitapadi,t.turadi,y.yazarad,y.yazarsoyad,i.atarih
    FROM ogrenci as o
    LEFT JOIN islem as i ON o.ogrno = i.ogrno
    LEFT JOIN kitap as k ON i.kitapno = k.kitapno
    LEFT JOIN yazar as y ON y.yazarno = k.yazarno
    LEFT JOIN tur as t on k.turno = t.turno
    WHERE i.atarih is null
    order by o.ograd desc;

13. 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    SELECT o.ograd,o.ogrsoyad,count(i.ogrno) as total_read
    FROM ogrenci as o
    INNER JOIN islem as i ON o.ogrno = i.ogrno
    WHERE o.sinif IN ('10A','10B')
    GROUP BY o.ograd,o.ogrsoyad
    order by total_read desc;

14. Tüm kitapların ortalama sayfa sayısını bulunuz. #AVG

    SELECT AVG(k.sayfasayisi) from kitap as k;

15. Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    SELECT k.\* FROM kitap k
    WHERE k.sayfasayisi > (SELECT AVG(k.sayfasayisi) from kitap as k);

16. Öğrenci tablosundaki öğrenci sayısını gösterin
    select count(o.ogrno) from ogrenci o;

17. Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
    select count(o.ogrno) as toplam_sayi from ogrenci o;

18. Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
    select distinct o.ograd from ogrenci o;

    select o.ograd,count(o.ograd) from
    ogrenci as o
    group by o.ograd;

19. En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
    select k.kitapadi,k.sayfasayisi
    from kitap as k
    order by k.sayfasayisi desc limit 1;

    select k.kitapadi,k.sayfasayisi
    from kitap as k
    where sayfasayisi = (select MAX(k.sayfasayisi) from kitap as k);

20. En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    select k.kitapadi,k.sayfasayisi
    from kitap as k
    order by k.sayfasayisi desc limit 1;

21. En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    select k.kitapadi,k.sayfasayisi
    from kitap as k
    order by k.sayfasayisi asc limit 1;

    select MIN(k.sayfasayisi)
    from kitap as k;

22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    select k.kitapadi,k.sayfasayisi
    from kitap as k
    order by k.sayfasayisi asc limit 1;

    select k.kitapadi,k.sayfasayisi
    from kitap as k
    where sayfasayisi = (select MIN(k.sayfasayisi) from kitap as k);

23. Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    select k.kitapadi,k.sayfasayisi
    from kitap as k INNER JOIN tur as t on t.turno = k.turno
    where t.turadi='Dram'
    order by k.sayfasayisi desc limit 1;

24. numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    select count(k.sayfasayisi) FROM ogrenci o INNER JOIN islem i
    ON o.ogrno = i.ogrno
    INNER JOIN kitap k on i.kitapno = k.kitapno
    where o.ogrno = 15;

25. İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    select o.ograd, count(o.ograd) from ogrenci o
    group by o.ograd
    order by o.ograd;

26. Her sınıftaki öğrenci sayısını bulunuz.

    select o.sinif, count(o.ogrno) from ogrenci o
    group by o.sinif
    order by o.sinif;

27. Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    select o.sinif,o.cinsiyet, count(o.ogrno) from ogrenci o
    group by o.sinif,o.cinsiyet
    order by o.sinif ,o.cinsiyet;

28. Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    select o.ograd,o.ogrsoyad, sum(k.sayfasayisi) as total_read from ogrenci o
    inner join islem i on o.ogrno = i.ogrno
    inner join kitap k on k.kitapno = i.kitapno
    group by o.ograd,o.ogrsoyad
    order by total_read desc;

29. Her öğrencinin okuduğu kitap sayısını getiriniz.

    select o.ograd,o.ogrsoyad, count(i.islemno) as total_book from ogrenci o
    inner join islem i on o.ogrno = i.ogrno
    group by o.ograd,o.ogrsoyad
    order by total_book desc;
