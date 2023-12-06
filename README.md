---
title: "Norrland Gum Dökümantasyon"
author: "Şebnem Perihan Gök"
date: "2023-11-22"

---

# Metodoloji

## Metaphlan çevresi kurulumu

```bash
conda create --name metaphlan -c bioconda metaphlan 
``` 

Alternatif olarak, conda çevre tanım dosyasını da kullanabiliriz:

```bash
conda env create -f envs/metaphlan.yaml
``` 

Çevre kurulumu ve asagıdaki asama arasında bir sey yaptım mı?

Bu asamada egitim92'den database ve sh dosyası cektim ve metaphlan icin kullanacagım inputları degisken olarak belirledim. daha sonra metaphlan isini gönderdim

Şuradayım: `/truba/home/egitim94` 

egitim92'den database klasorunu(`-r`) kopyala

```bash
cp -r /truba/home/egitim92/metaphlan_databases_v3.0/ .
``` 

bu üç kesme isaretini her komutun basına mı koymalıyım?


egitim92'den sh dosyasını `norrland_gum` icine kopyala

`cd norrland_gum/` 

`cp /truba/home/egitim92/nrrldn_gum/metaphlan_test.sh .`


`results/processed` içinde gz ile biten dosyaları listele

`ls results/processed/*gz`  

`gz` dosyalarını `fastq.txt` içine yazdır


```bash
ls results/processed/*gz > fastq.txt  

nano metaphlan_test.sh
```

fastq.txt’teki FILE(lar) için FILE değişkeni ata (her bi satırın bir FILE olduğunu nasıl biliyo?)
cat demesek neden olmuyor

Tek bir satırda yazmak istersek:

```
for FILE in $(cat fastq.txt); do echo ${FILE}; done 
``` 

Betik içersisinde böyle:

```bash
for FILE in $(cat fastq.txt)
do
	echo ${FILE}
done 
``` 

Yukarıdaki `for` döngüsünde yapılan şey aslında şu:

1. önce `cat fastq.txt` komuıtu ile dosyayı okuyoruz
2. daha sonra bu dosyayı `for` döngüsüne satır satır veriyoruz
3. önce birinci satırı `FILE` değişkeni içerisinde saklıyoruz
4. bu `FILE` içerisindeki bilgi kullanarak `do` kelimesinden sonraki işlemi gerçekleştiriyozu
5. burada `echo ${FILE}` bize, `FILE` değişkeni içerisindeki metin bilgisini ekrana yazdırmamızı sağlıyor.
6. bu iş bittikten sonra, `fastq.txt` dosyası içerisindeki ikinci satırı `FILE` değişkeni içerisine atıoyroız. ve tekrar `do` klelimesinden sonra geelen işlemi tekrar ediyoruz
7. Dosyuanın sonuna geldiğimizde döngüyü sonlandıruyzü

Dosyanın içeriğine bakalım:

```bash
cat fastq.txt
```

İşlerimizi bu şekilde gönderiyıruz. Önce işi betiğimizi kontrol edelimö. Gerçekten de `sbatch ` komuotlarını düzgün bir şekjilde gönderebiliyor muyuz?

```bash
for FILE in $(cat fastq.txt)
do 
	echo "sbatch metaphlan_test.sh ${FILE}"
done
```

Şimdi işimizi gönderlim:

```bash
for FILE in $(cat fastq.txt)
do
	sbatch metaphlan_test.sh ${FILE}
done
```

Son olarak işlerimizi kontrol edelim:

```bash
squeue 
``` 

