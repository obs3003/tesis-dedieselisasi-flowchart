```mermaid
flowchart LR;

%% --- KOLOM KIRI (pipeline utama) ---
subgraph L[" "]
direction TB
S((Mulai)) --> IO[/Studi literatur<br/>+ Pengumpulan data/];
IO --> P1[Data cleaning &<br/>pre-processing (Z-score)];
P1 --> D1{Missing/Outlier<br/>OK?};
D1 -- Tidak --> P1;
D1 -- Ya --> P2[Clustering (K-means/CLARA)<br/>(opsional PCA)];
P2 --> P3[Validasi cluster<br/>(Silhouette/CH/DB)];
P3 --> D2{Cluster valid?};
D2 -- Tidak --> P2;
D2 -- Ya --> P4[Hitung indeks implementasi<br/>PRI (GHI) & GCI (jarak+depth)];
P4 --> P5[Definisi 7 kriteria<br/>(benefit/cost)];
P5 --> P6[AHP: bobot kriteria<br/>+ uji CR];
P6 --> D3{CR ≤ 0,10?};
D3 -- Tidak --> P6;
D3 -- Ya --> A((A));
end

%% --- KOLOM KANAN (ranking + robustness) ---
subgraph R[" "]
direction TB
A --> T1[TOPSIS: normalisasi<br/>+ bobot AHP + CC];
T1 --> T2[Robustness & benchmarking<br/>(ablation + Monte Carlo bobot)];
T2 --> D4{Ranking stabil?<br/>(Top-10 overlap tinggi)};
D4 -- Ya --> OUT[/Output akhir:<br/>tabel + insight/];
OUT --> E((Selesai));
D4 -- Tidak --> FIX[Kalibrasi indeks/kriteria<br/>atau cek kualitas data];
end

%% loop balik rapi ke langkah indeks/kriteria
FIX --> P4;
```
