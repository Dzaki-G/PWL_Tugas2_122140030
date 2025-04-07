# Microservices Order System

Proyek ini merupakan simulasi sistem pemesanan berbasis microservices, terdiri dari beberapa layanan (service) yang berjalan pada port yang berbeda, serta menggunakan Postman untuk pengujian alur proses.

## Service dan Port

| Service            | Port  |
|--------------------|-------|
| Order Service      | 8000  |
| Payment Service    | 8001  |
| Shipping Service   | 8002  |
| Orchestrator       | 8003  |

## Tools

- [Postman](https://www.postman.com/) – untuk simulasi alur proses antar service

## Alur Proses

1. **Pembuatan Order**  
   - Endpoint: `POST /order`
   - Menghasilkan `order_id` dan jumlah barang yang dipesan.

2. **Pembayaran Order**  
   - Endpoint: `POST /payment`
   - Mengubah status order menjadi `SUCCESS` dan mengisi `amount` (jumlah pembayaran).

3. **Pengiriman Barang**  
   - Endpoint: `POST /ship-order`
   - Mengirimkan nama item (misalnya `GPU`) berdasarkan `order_id`.

4. **Compensation Action (Jika Terjadi Kegagalan Transaksi)**  
   Jika terjadi error pada salah satu service, orchestrator akan mengembalikan sistem ke kondisi semula:
    - **Shipping Service Cancelled**: Jika pengiriman gagal, Shipping Service akan dibatalkan.
    - **Refund Payment**: Payment Service akan menghapus jumlah produk yang telah dibayar dan mengembalikan dana.
    - **Cancel Order**: Order Service akan membatalkan pesanan yang telah dibuat.


Menjalankan service masing-masing pada terminal yang terpisah menggunakan perintah:
```bash
go run main.go
