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
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
			SELECT o.ograd,o.ogrsoyad, i.atarih
				FROM ogrenci o
				WHERE o.ogrno IN (
					  SELECT i.ogrno
						  FROM islem i
  
								);


	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
			SELECT k.kitapadi, t.turadi
				FROM kitap k
					WHERE k.turno IN (
  						SELECT turno
  							FROM tur t
  								WHERE turadi = 'Fıkra' OR turadi = 'hikaye'
									);
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
			 SELECT o.ograd, o.ogrsoyad ,atarih
				FROM ogrenci as o 
					INNER JOIN islem as i ON o.ogrno = i.ogrno
				

	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
			
			 SELECT k.kitapadi , t.turadi
				FROM kitap as k 
					INNER JOIN tur as t ON k.turno = t.turno
						WHERE t.turadi = "Fıkra" OR t.turadi ="hikaye"

	
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
                       SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				INNER JOIN kitap k
					ON i.kitapno = k.kitapno
                               WHERE o.sinif IN('10B','10C')
                              ORDER BY o.ograd ASC

			
	
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	
	8) Kitap almayan öğrencileri listeleyin.
			SELECT o.ograd, o.ogrsoyad, i.tarih
				FROM ogrenci as o
					LEFT JOIN islem as i
					ON o.ogrno = i.ogrno
					WHERE i.ogrno IS null
	
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
			SELECT i.kitapno, k.kitapadi, count(i.kitapno) as TOTAL
				FROM islem as i
					INNER JOIN kitap as k
					ON i.kitapno = k.kitapno
					GROUP BY i.kitapno,k.kitapadi
					ORDER BY total ASC
	
	
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

			SELECT i.kitapno, k.kitapadi, count(i.kitapno) as TOTAL
				FROM kitap as k
					LEFT JOIN islem as i
					ON k.kitapno = i.kitapno
					GROUP BY i.kitapno,k.kitapadi
					ORDER BY total ASC


	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
			SELECT o.ograd, o.ogrsoyad, k.kitapadi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				INNER JOIN kitap k
					ON i.kitapno = k.kitapno
                                         
                                   

			
	
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
		
		SELECT o.ograd, o.ogrsoyad, k.kitapadi, t.turadi, y.yazarad, y.yazarsoyad, i.atarih
			FROM ogrenci as o
				LEFT JOIN islem as i
					ON o.ogrno = i.ogrno
				LEFT JOIN kitap as k
					ON i.kitapno = k.kitapno
				LEFT JOIN yazar as y
					ON k.yazarno = y.yazarno
				LEFT JOIN tur as t
					ON y.turno = t.turno
						ORDER BY  k.kitapadi DESC
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
		
		SELECT o.ograd, o.ogrsoyad, COUNT(k.kitapadi) AS okudugu_kitap_sayisi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				INNER JOIN kitap k
					ON i.kitapno = k.kitapno
				WHERE o.sinif IN ('10A', '10B')
					GROUP BY o.ograd, o.ogrsoyad
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
			SELECT  AVG(k.sayfasayisi)
				FROM kitap k
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
			SELECT * FROM kitap k
				WHERE k.sayfasayisi > (
  						SELECT AVG(k.sayfasayisi)
  							FROM kitap k
)
					ORDER BY k.sayfasayisi DESC;
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
			
			SELECT COUNT(o.ogrno) as ogrenci_sayisi FROM ogrenci o
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
			SELECT COUNT(o.ogrno) as toplam_sayi FROM ogrenci o
	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
			SELECT COUNT(DISTINCT o.ograd) as farklı_ogrenci_sayisi FROM ogrenci o
	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
			SELECT k.kitapadi, k.sayfasayisi FROM kitap k
				ORDER BY k.sayfasayisi DESC
					LIMIT 1
	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
			SELECT k.kitapadi, k.sayfasayisi FROM kitap k
				ORDER BY k.sayfasayisi DESC
				LIMIT 1
	
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
			SELECT k.kitapadi, k.sayfasayisi FROM kitap k
				ORDER BY k.sayfasayisi ASC
					LIMIT 1
	
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
			SELECT k.kitapadi, k.sayfasayisi FROM kitap k
				ORDER BY k.sayfasayisi DESC
					LIMIT 1
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
			SELECT k.kitapadi, k.sayfasayisi,t.turadi FROM kitap k
				LEFT JOIN tur t
					ON k.turno = t.turno
				WHERE t.turadi ='Dram'
					ORDER BY k.sayfasayisi DESC
					LIMIT 1
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
			SELECT o.ogrno , o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) AS okudugu_sayfa_sayisi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				INNER JOIN kitap k
					ON i.kitapno = k.kitapno
				WHERE o.ogrno = 15
					
	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
			SELECT COUNT(o.ograd), o.ograd FROM ogrenci o
				GROUP BY o.ograd

	
	26) Her sınıftaki öğrenci sayısını bulunuz.
			SELECT o.sinif, COUNT(o.ogrno) FROM ogrenci o
				GROUP BY o.sinif
			
	
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
			SELECT o.sinif, SUM(o.cinsiyet='K') as K,SUM(o.cinsiyet='E') as E   
				 FROM ogrenci o
					GROUP BY o.sinif
	
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
			SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) AS okudugu_sayfa_sayisi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				INNER JOIN kitap k
					ON i.kitapno = k.kitapno
                                         GROUP BY o.ograd, o.ogrsoyad
                                   ORDER BY okudugu_sayfa_sayisi DESC
	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
			SELECT o.ograd, o.ogrsoyad, SUM(i.kitapno) AS okudugu_kitap_sayisi
			FROM ogrenci o
				INNER JOIN islem i
					ON o.ogrno = i.ogrno
				
                                         GROUP BY o.ograd, o.ogrsoyad
			
