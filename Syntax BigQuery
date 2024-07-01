CREATE TABLE kimia_farma.tabel_analisa AS
WITH gross_profit AS -- membuat tabel sementara bernama gross_profit untuk menghitung persentase_gross_laba
  (SELECT
    product_id,
    (CASE
      WHEN price <= 50000 THEN 0.10
      WHEN price > 50000 AND price <= 100000 THEN 0.15
      WHEN price > 100000 AND price <= 300000 THEN 0.20
      WHEN price > 300000 AND price <= 500000 THEN 0.25
      ELSE 0.30
    END) AS persentase_gross_laba
  FROM kimia_farma.kf_product
  )
SELECT
    t.transaction_id,
    t.date,
    t.branch_id,
    kc.branch_name,
    kc.kota,
    kc.provinsi,
    kc.rating AS rating_cabang,
    t.customer_name,
    t.product_id,
    p.product_name,
    p.product_category,
    p.price AS actual_price,
    t.discount_percentage,
    g.persentase_gross_laba AS persentase_gross_laba_from_gross_profit,
    t.price * (1 - t.discount_percentage) AS nett_sales,
    (p.price*g.persentase_gross_laba) - (p.price * t.discount_percentage) AS nett_profit,
    t.rating AS rating_transaksi
FROM
    kimia_farma.kf_final_transaction t
LEFT JOIN
    kimia_farma.kf_kantor_cabang kc ON t.branch_id = kc.branch_id
LEFT JOIN
    kimia_farma.kf_product p ON t.product_id = p.product_id
JOIN
  gross_profit AS g ON t.product_id = g.product_id
;
