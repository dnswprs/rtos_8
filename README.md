# Percobaan Mengeliminasi Resource Contention RTOS dengan Mutual Exclusion (Mutex)
## Tentang Proyek
Proyek embedded system ini menggunakan FreeRTOS untuk mengontrol LED dengan mekanisme sinkronisasi kompleks. Inti dari sistem adalah manajemen sumber daya bersama melalui mekanisme semaphore.
Semaphore adalah mekanisme kritis dalam RTOS yang mengatur akses sumber daya terbatas antarthread. Ia mencegah kondisi balapan (race condition) dengan membatasi akses ke variabel atau periferal bersama, menggunakan binary semaphore atau counting semaphore.

## Komponen Task
### Proyek ini menggunakan tiga task yang berbeda:
- GreenLedTask: Task ini menghidupkan dan mematikan LED hijau secara bergantian.
- RedLedTask: Task ini menghidupkan dan mematikan LED merah secara bergantian.
- orangeLedTask: Task ini menghidupkan dan mematikan LED oranye secara bergantian.

## Cara Kerja Program
### Inisialisasi Sistem
1. Melakukan konfigurasi clock sistem
2. Menginisialisasi GPIO untuk LED
3. Nonatktifkan semua LED pada awal program
4. Menginisialisasi kernel RTOS
5. Membuat dan memulai thread-thread

### Logika Program
1. Terdapat tiga tugas (task) utama: FlashGreenLedTask, FlashRedLedTask, dan FlashOrangeLedTask.
2. Sebuah semaphore bernama CriticalResourceSemaphore digunakan untuk mengontrol akses ke sumber daya bersama (shared resource).
3. FlashGreenLedTask dan FlashRedLedTask bersaing untuk mendapatkan akses ke sumber daya bersama:
  - Kedua tugas ini mencoba untuk mendapatkan semaphore dengan osSemaphoreWait().
  - Jika berhasil mendapatkan semaphore, tugas tersebut akan memanggil fungsi AccessSharedData().
  - Setelah selesai, tugas melepaskan semaphore dengan osSemaphoreRelease().
4. Fungsi AccessSharedData() mensimulasikan akses ke sumber daya bersama:
5. FlashOrangeLedTask berjalan secara independen, tidak menggunakan semaphore, dan hanya mengedipkan LED Oranye.
6. Penggunaan semaphore memastikan bahwa hanya satu tugas yang dapat mengakses sumber daya bersama pada satu waktu, mencegah kondisi balapan (race condition) dan memastikan integritas data.

### Diagram Timing Program
![image](https://github.com/user-attachments/assets/ba465353-f51c-4200-a4d0-6c9c2c450dd0)

## Konfigurasi Hardware
### Program menggunakan konfigurasi GPIO berikut:
 - GPIOE: LED Oranye dan Biru
 - GPIOB: LED Hijau dan Merah

## Catatan Pengembangan
- Program menggunakan HAL (Hardware Abstraction Layer) STM32

## Percobaan