Bài 1: Mảng cơ bản ⭐
Nhập mảng n phần tử. Tính min, max, trung bình, tổng. Không dùng STL.
#include <iostream>
using namespace std;

int main() {
    int n;

    // Nhap so phan tu
    cout << "Nhap n = ";
    cin >> n;

    int a[100];

    // Nhap mang
    cout << "Nhap cac phan tu:\n";
    for (int i = 0; i < n; i++) {
        cout << "a[" << i << "] = ";
        cin >> a[i];
    }

    // Khoi tao
    int min = a[0];
    int max = a[0];
    int tong = 0;

    // Xu ly
    for (int i = 0; i < n; i++) {
        tong += a[i];

        if (a[i] < min)
            min = a[i];

        if (a[i] > max)
            max = a[i];
    }

    // Tinh trung binh
    float tbc = (float)tong / n;

    // Xuat ket qua
    cout << "\nTong = " << tong;
    cout << "\nMin = " << min;
    cout << "\nMax = " << max;
    cout << "\nTrung binh cong = " << tbc;

    return 0;
}
Bài 2: Mảng 2D ⭐⭐
Nhân 2 ma trận n×n. Tính định thức ma trận 3×3. Hiển thị đẹp.
#include <iostream>
#include <iomanip>
using namespace std;

// Ham nhap ma tran
void NhapMaTran(int a[][10], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << "a[" << i << "][" << j << "] = ";
            cin >> a[i][j];
        }
    }
}

// Ham xuat ma tran
void XuatMaTran(int a[][10], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << setw(6) << a[i][j];
        }
        cout << endl;
    }
}

// Nhan 2 ma tran
void NhanMaTran(int a[][10], int b[][10], int c[][10], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {

            c[i][j] = 0;

            for (int k = 0; k < n; k++) {
                c[i][j] += a[i][k] * b[k][j];
            }
        }
    }
}

// Tinh dinh thuc ma tran 3x3
int DinhThuc3x3(int a[][10]) {
    int det;

    det =
        a[0][0] * (a[1][1] * a[2][2] - a[1][2] * a[2][1])
        - a[0][1] * (a[1][0] * a[2][2] - a[1][2] * a[2][0])
        + a[0][2] * (a[1][0] * a[2][1] - a[1][1] * a[2][0]);

    return det;
}

int main() {
    int n;

    cout << "Nhap cap ma tran n = ";
    cin >> n;

    int A[10][10], B[10][10], C[10][10];

    // Nhap ma tran A
    cout << "\nNhap ma tran A:\n";
    NhapMaTran(A, n);

    // Nhap ma tran B
    cout << "\nNhap ma tran B:\n";
    NhapMaTran(B, n);

    // Xuat ma tran
    cout << "\nMa tran A:\n";
    XuatMaTran(A, n);

    cout << "\nMa tran B:\n";
    XuatMaTran(B, n);

    // Nhan ma tran
    NhanMaTran(A, B, C, n);

    cout << "\nMa tran C = A * B:\n";
    XuatMaTran(C, n);

    // Tinh dinh thuc neu la ma tran 3x3
    if (n == 3) {
        int dt = DinhThuc3x3(A);

        cout << "\nDinh thuc ma tran A = " << dt << endl;
    }
    else {
        cout << "\nChi tinh dinh thuc cho ma tran 3x3.\n";
    }

    return 0;
}
Bài 3: Con trỏ & cấp phát động ⭐⭐
Cài đặt mảng động tự resize (như std::vector đơn giản). Hỗ trợ push_back, pop_back, at(i).
#include <iostream>
using namespace std;

class DynamicArray {
private:
    int* data;      // Mang dong
    int size;       // So phan tu hien tai
    int capacity;   // Kich thuoc cap phat

    // Ham mo rong mang
    void resize(int newCapacity) {
        int* temp = new int[newCapacity];

        for (int i = 0; i < size; i++) {
            temp[i] = data[i];
        }

        delete[] data;
        data = temp;
        capacity = newCapacity;
    }

public:
    // Constructor
    DynamicArray() {
        capacity = 2;
        size = 0;
        data = new int[capacity];
    }

    // Destructor
    ~DynamicArray() {
        delete[] data;
    }

    // Them phan tu vao cuoi
    void push_back(int value) {

        // Neu day mang -> tang kich thuoc
        if (size == capacity) {
            resize(capacity * 2);
        }

        data[size] = value;
        size++;
    }

    // Xoa phan tu cuoi
    void pop_back() {
        if (size > 0) {
            size--;
        }
        else {
            cout << "Mang rong!\n";
        }
    }

    // Truy cap phan tu
    int at(int index) {

        if (index < 0 || index >= size) {
            cout << "Chi so khong hop le!\n";
            return -1;
        }

        return data[index];
    }

    // Lay so phan tu
    int getSize() {
        return size;
    }

    // Hien thi mang
    void print() {
        cout << "[ ";

        for (int i = 0; i < size; i++) {
            cout << data[i] << " ";
        }

        cout << "]\n";
    }
};

int main() {

    DynamicArray arr;

    // Them phan tu
    arr.push_back(10);
    arr.push_back(20);
    arr.push_back(30);
    arr.push_back(40);

    cout << "Mang sau push_back:\n";
    arr.print();

    // Truy cap phan tu
    cout << "\nPhan tu tai vi tri 2: ";
    cout << arr.at(2) << endl;

    // Xoa phan tu cuoi
    arr.pop_back();

    cout << "\nMang sau pop_back:\n";
    arr.print();

    cout << "\nSo phan tu hien tai: ";
    cout << arr.getSize() << endl;

    return 0;
}
Bài 4: 🔥 Dự Án Mini — Student Score Manager ⭐⭐⭐
Cảm hứng: BaiTapTongHop — Quản lý sinh viên (DSALab)

Xây dựng hệ thống quản lý điểm sinh viên bằng mảng động:

Thêm / xóa / sửa sinh viên (tên, MSSV, điểm)
Sắp xếp theo điểm (dùng Selection Sort hoặc Bubble Sort)
Tìm kiếm theo tên hoặc MSSV (Linear Search)
Thống kê: điểm cao nhất, thấp nhất, trung bình lớp
Xuất danh sách ra file diem_sinhvien.txt
=== QUẢN LÝ ĐIỂM SINH VIÊN ===
1. Thêm sinh viên
2. Xóa sinh viên
3. Tìm kiếm
4. Xếp hạng lớp
5. Xuất báo cáo
0. Thoát
#include <iostream>
#include <iomanip>
#include <fstream>
using namespace std;

// ================== STRUCT ==================
struct SinhVien {
    string mssv;
    string ten;
    float diem;
};

// ================== CLASS QUAN LY ==================
class QuanLy {
private:
    SinhVien* ds;
    int n;
    int cap;

    void resize() {
        cap *= 2;
        SinhVien* tmp = new SinhVien[cap];
        for (int i = 0; i < n; i++)
            tmp[i] = ds[i];
        delete[] ds;
        ds = tmp;
    }

    int findIndex(string key) {
        for (int i = 0; i < n; i++) {
            if (ds[i].mssv == key || ds[i].ten == key)
                return i;
        }
        return -1;
    }

public:
    QuanLy() {
        cap = 2;
        n = 0;
        ds = new SinhVien[cap];
    }

    ~QuanLy() {
        delete[] ds;
    }

    // ===== 1. THEM =====
    void them() {
        if (n == cap) resize();

        cin.ignore();
        cout << "Nhap MSSV: ";
        getline(cin, ds[n].mssv);

        cout << "Nhap ten: ";
        getline(cin, ds[n].ten);

        cout << "Nhap diem: ";
        cin >> ds[n].diem;

        n++;
    }

    // ===== 2. XOA =====
    void xoa() {
        cin.ignore();
        string key;
        cout << "Nhap MSSV can xoa: ";
        getline(cin, key);

        int pos = findIndex(key);

        if (pos == -1) {
            cout << "Khong tim thay!\n";
            return;
        }

        for (int i = pos; i < n - 1; i++)
            ds[i] = ds[i + 1];

        n--;

        cout << "Da xoa!\n";
    }

    // ===== 3. SUA =====
    void sua() {
        cin.ignore();
        string key;
        cout << "Nhap MSSV can sua: ";
        getline(cin, key);

        int pos = findIndex(key);

        if (pos == -1) {
            cout << "Khong tim thay!\n";
            return;
        }

        cout << "Nhap diem moi: ";
        cin >> ds[pos].diem;

        cout << "Cap nhat thanh cong!\n";
    }

    // ===== 4. TIM KIEM =====
    void timKiem() {
        cin.ignore();
        string key;
        cout << "Nhap MSSV hoac ten: ";
        getline(cin, key);

        bool found = false;

        for (int i = 0; i < n; i++) {
            if (ds[i].mssv == key || ds[i].ten == key) {
                cout << ds[i].mssv << " | "
                     << ds[i].ten << " | "
                     << ds[i].diem << "\n";
                found = true;
            }
        }

        if (!found)
            cout << "Khong tim thay!\n";
    }

    // ===== 5. SAP XEP (Bubble Sort) =====
    void sapXep() {
        for (int i = 0; i < n - 1; i++) {
            for (int j = n - 1; j > i; j--) {
                if (ds[j].diem > ds[j - 1].diem) {
                    swap(ds[j], ds[j - 1]);
                }
            }
        }

        cout << "Da sap xep theo diem giam dan!\n";
    }

    // ===== 6. THONG KE =====
    void thongKe() {
        if (n == 0) return;

        float max = ds[0].diem;
        float min = ds[0].diem;
        float sum = 0;

        for (int i = 0; i < n; i++) {
            if (ds[i].diem > max) max = ds[i].diem;
            if (ds[i].diem < min) min = ds[i].diem;
            sum += ds[i].diem;
        }

        cout << "Diem cao nhat: " << max << "\n";
        cout << "Diem thap nhat: " << min << "\n";
        cout << "Diem trung binh: " << sum / n << "\n";
    }

    // ===== 7. XUAT DANH SACH =====
    void xuat() {
        cout << left
             << setw(10) << "MSSV"
             << setw(20) << "TEN"
             << setw(10) << "DIEM\n";

        for (int i = 0; i < n; i++) {
            cout << setw(10) << ds[i].mssv
                 << setw(20) << ds[i].ten
                 << setw(10) << ds[i].diem << "\n";
        }
    }

    // ===== 8. XUAT FILE =====
    void xuatFile() {
        ofstream f("diem_sinhvien.txt");

        for (int i = 0; i < n; i++) {
            f << ds[i].mssv << " "
              << ds[i].ten << " "
              << ds[i].diem << "\n";
        }

        cout << "Da xuat file!\n";
    }
};

// ================== MAIN MENU ==================
int main() {
    QuanLy ql;
    int chon;

    do {
        cout << "\n=== QUAN LY DIEM SINH VIEN ===\n";
        cout << "1. Them sinh vien\n";
        cout << "2. Xoa sinh vien\n";
        cout << "3. Tim kiem\n";
        cout << "4. Xep hang lop\n";
        cout << "5. Xuat bao cao\n";
        cout << "6. Sua diem\n";
        cout << "7. Thong ke\n";
        cout << "8. Xuat file\n";
        cout << "0. Thoat\n";
        cout << "Chon: ";
        cin >> chon;

        switch (chon) {
            case 1: ql.them(); break;
            case 2: ql.xoa(); break;
            case 3: ql.timKiem(); break;
            case 4: ql.sapXep(); break;
            case 5: ql.xuat(); break;
            case 6: ql.sua(); break;
            case 7: ql.thongKe(); break;
            case 8: ql.xuatFile(); break;
            case 0: cout << "Thoat!\n"; break;
            default: cout << "Sai lua chon!\n";
        }

    } while (chon != 0);

    return 0;
}
