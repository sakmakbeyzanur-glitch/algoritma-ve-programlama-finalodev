#include <stdio.h>
#include <math.h>

int main(void)
{
    float urun_tutari, odenen_para;
    int para_ustu;
    int adet, toplam_kupur = 0;
    int i;

    // Kupurler kurus cinsinden buyukten kucuge siralanmistir.
    int kupurler[] = {
        20000, 10000, 5000, 2000, 1000, 500,
        100, 50, 25, 10, 5, 1
    };

    const char *kupur_isimleri[] = {
        "200 TL banknot",
        "100 TL banknot",
        "50 TL banknot",
        "20 TL banknot",
        "10 TL banknot",
        "5 TL banknot",
        "1 TL madeni para",
        "50 kurus",
        "25 kurus",
        "10 kurus",
        "5 kurus",
        "1 kurus"
    };

    int kupur_sayisi = sizeof(kupurler) / sizeof(kupurler[0]);

    printf("Urun tutarini TL olarak giriniz: ");

    if (scanf("%f", &urun_tutari) != 1 ||
        !isfinite(urun_tutari) || urun_tutari < 0) {
        printf("Gecersiz urun tutari!\n");
        return 1;
    }

    printf("Odenen parayi TL olarak giriniz: ");

    if (scanf("%f", &odenen_para) != 1 ||
        !isfinite(odenen_para) || odenen_para < 0) {
        printf("Gecersiz odeme tutari!\n");
        return 1;
    }

    if (odenen_para < urun_tutari) {
        printf("Odeme yetersiz!\n");
        printf("Eksik tutar: %.2f TL\n",
               urun_tutari - odenen_para);
        return 0;
    }

    // Para ustunu kurus cinsine cevir ve yuvarla.
    para_ustu = (int)roundf(
        (odenen_para - urun_tutari) * 100.0f
    );

    printf("\nPara ustu: %.2f TL\n", para_ustu / 100.0);
    printf("Para ustu dagilimi:\n");

    // Acgozlu algoritma: Buyuk kupurden kucuge ilerle.
    for (i = 0; i < kupur_sayisi; i++) {
        adet = para_ustu / kupurler[i];

        if (adet > 0) {
            printf("%s: %d adet\n", kupur_isimleri[i], adet);
            toplam_kupur += adet;
        }

        para_ustu = para_ustu % kupurler[i];
    }

    printf("\nToplam kupur sayisi: %d\n", toplam_kupur);

    return 0;
}
