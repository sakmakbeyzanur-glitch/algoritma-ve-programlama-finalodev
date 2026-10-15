/******************************************************************************
 * Akıllı Kasa Programı (C Dili)
 * 
 * Bu program; ürün tutarını ve müşterinin ödediği parayı float olarak alır,
 * hassasiyet kaybını önlemek için kuruş cinsinden tam sayılarla hesaplama yapar
 * ve müşteriye verilecek para üstünü en az sayıda banknot/madeni para ile dağıtarak
 * toplam küpür adedini ekrana yazdırır.
 ******************************************************************************/

#include <stdio.h>
#include <math.h> // round() fonksiyonu için (Derlerken -lm baytı gerekebilir)

void akilli_kasa(float urun_tutari, float odenen_para) {
    // Para üstü hesabı (float)
    float para_ustu_tl = odenen_para - urun_tutari;

    // Doğru yuvarlama yöntemi (round kullanarak kayan nokta hassasiyet kaybını önleme)
    int toplam_kurus = (int)round(para_ustu_tl * 100);

    // Küpür ve madeni para birimleri ve isimleri (kuruş cinsinden büyükten küçüğe)
    int degerler[] = {20000, 10000, 5000, 2000, 1000, 500, 100, 50, 25, 10, 5, 1};
    const char *isimler[] = {
        "200 TL Banknot", "100 TL Banknot", "50 TL Banknot", "20 TL Banknot", 
        "10 TL Banknot", "5 TL Banknot", "1 TL Madeni Para", "50 Kurus", 
        "25 Kurus", "10 Kurus", "5 Kurus", "1 Kurus"
    };
    int boyut = 12;

    int kalan = toplam_kurus;
    int toplam_kupur_sayisi = 0;

    printf("--- KASA ISLEM RAPORU ---\n");
    printf("Urun Tutari      : %.2f TL\n", urun_tutari);
    printf("Odenen Para      : %.2f TL\n", odenen_para);
    printf("Toplam Para Ustu : %.2f TL (%d Kurus)\n\n", para_ustu_tl, toplam_kurus);
    printf("Para Ustu Dagilimi:\n");

    // En az sayıda küpür/madeni para ile dağılım hesabı
    for (int i = 0; i < boyut; i++) {
        if (kalan >= degerler[i]) {
            int adet = kalan / degerler[i];
            kalan = kalan % degerler[i];
            toplam_kupur_sayisi += adet;
            printf("  - %d adet %s\n", adet, isimler[i]);
        }
    }

    // Toplam küpür/madeni para sayısı
    printf("\n==> Toplam Verilen Kupur/Madeni Para Sayisi: %d\n", toplam_kupur_sayisi);
    printf("-------------------------------------------\n\n");
}

int main() {
    // Örnek Test Senaryosu (Ürün: 18.70 TL, Ödenen: 20.00 TL)
    akilli_kasa(18.70f, 20.00f);
    
    return 0;
}
