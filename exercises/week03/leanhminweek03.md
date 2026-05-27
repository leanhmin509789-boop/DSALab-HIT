Bài 1: Linear Search ⭐
Tìm kiếm tuyến tính trên mảng số nguyên và mảng chuỗi. Đếm số bước so sánh.
#include <iostream>
#include <string>
using namespace std;

int linearSearchInt(int a[], int n, int x, int &steps) {
    steps = 0;
    for (int i = 0; i < n; i++) {
        steps++;
        if (a[i] == x) return i;
    }
    return -1;
}

int linearSearchString(string a[], int n, string x, int &steps) {
    steps = 0;
    for (int i = 0; i < n; i++) {
        steps++;
        if (a[i] == x) return i;
    }
    return -1;
}
Bài 2: Binary Search ⭐⭐
Cài đặt Binary Search iterative + recursive. Tìm vị trí đầu tiên và cuối cùng của phần tử trùng.
int binaryIter(int a[], int n, int x) {
    int l = 0, r = n - 1;
    while (l <= r) {
        int m = (l + r) / 2;
        if (a[m] == x) return m;
        else if (a[m] < x) l = m + 1;
        else r = m - 1;
    }
    return -1;
}

int binaryRec(int a[], int l, int r, int x) {
    if (l > r) return -1;

    int m = (l + r) / 2;

    if (a[m] == x) return m;
    if (a[m] < x) return binaryRec(a, m + 1, r, x);
    return binaryRec(a, l, m - 1, x);
}

int firstPos(int a[], int n, int x) {
    int l = 0, r = n - 1, res = -1;

    while (l <= r) {
        int m = (l + r) / 2;

        if (a[m] == x) {
            res = m;
            r = m - 1;
        }
        else if (a[m] < x) l = m + 1;
        else r = m - 1;
    }
    return res;
}

int lastPos(int a[], int n, int x) {
    int l = 0, r = n - 1, res = -1;

    while (l <= r) {
        int m = (l + r) / 2;

        if (a[m] == x) {
            res = m;
            l = m + 1;
        }
        else if (a[m] < x) l = m + 1;
        else r = m - 1;
    }
    return res;
}
Bài 3: So sánh hiệu năng ⭐⭐
Đo thời gian tìm kiếm với n = 10.000, 100.000, 1.000.000 phần tử. Vẽ bảng so sánh.
#include <iostream>
#include <ctime>
using namespace std;

bool linear(int a[], int n, int x) {
    for (int i = 0; i < n; i++)
        if (a[i] == x) return true;
    return false;
}

bool binary(int a[], int n, int x) {
    int l = 0, r = n - 1;

    while (l <= r) {
        int m = (l + r) / 2;
        if (a[m] == x) return true;
        else if (a[m] < x) l = m + 1;
        else r = m - 1;
    }
    return false;
}

void test(int n) {
    int *a = new int[n];

    for (int i = 0; i < n; i++)
        a[i] = i;

    clock_t t1 = clock();
    linear(a, n, n - 1);
    clock_t t2 = clock();

    clock_t t3 = clock();
    binary(a, n, n - 1);
    clock_t t4 = clock();

    cout << "\nN = " << n;
    cout << "\nLinear: " << (double)(t2 - t1) / CLOCKS_PER_SEC;
    cout << "\nBinary: " << (double)(t4 - t3) / CLOCKS_PER_SEC << "\n";

    delete[] a;
}
Bài 4: 🔥 Dự Án Mini — Smart Search Engine ⭐⭐⭐
Cảm hứng: Brute Force Search — algorithm-visualizer

Xây dựng hệ thống tìm kiếm danh bạ điện thoại:

Tìm theo tên: Linear Search (hỗ trợ tìm kiếm mờ — chứa chuỗi con)
Tìm theo số điện thoại: Binary Search (sau khi sort theo SĐT)
Thống kê: hiển thị số bước so sánh, thời gian tìm kiếm
Gợi ý: nếu không tìm thấy, gợi ý 3 tên gần giống nhất
Nhập tên cần tìm: "Minh"
→ Tìm thấy 3 kết quả:
   1. Nguyễn Văn Minh   - 0901234567
   2. Trần Thị Minh Anh - 0912345678
   3. Lê Minh Tuấn      - 0923456789
   (Đã so sánh 15/50 phần tử — 0.002ms)
#include <iostream>
#include <iomanip>
#include <string>
#include <ctime>
using namespace std;

struct Contact {
    string name;
    string phone;
};

// ===== LINEAR SEARCH (fuzzy name) =====
void searchName(Contact a[], int n, string key, int &steps) {
    steps = 0;

    cout << "\nKet qua tim kiem:\n";

    for (int i = 0; i < n; i++) {
        steps++;

        if (a[i].name.find(key) != string::npos) {
            cout << a[i].name << " - " << a[i].phone << "\n";
        }
    }

    cout << "(So lan so sanh: " << steps << ")\n";
}

// ===== BINARY SEARCH PHONE =====
int cmp(Contact a, Contact b) {
    return a.phone < b.phone;
}

int binaryPhone(Contact a[], int n, string key) {
    int l = 0, r = n - 1;

    while (l <= r) {
        int m = (l + r) / 2;

        if (a[m].phone == key) return m;
        if (a[m].phone < key) l = m + 1;
        else r = m - 1;
    }

    return -1;
}

// ===== SORT PHONE =====
void sortPhone(Contact a[], int n) {
    for (int i = 0; i < n - 1; i++)
        for (int j = i + 1; j < n; j++)
            if (a[i].phone > a[j].phone)
                swap(a[i], a[j]);
}

// ===== GỢI Ý =====
void suggest(Contact a[], int n, string key) {
    cout << "\nGoi y gan dung:\n";

    int count = 0;

    for (int i = 0; i < n && count < 3; i++) {
        if (a[i].name.find(key.substr(0, 2)) != string::npos) {
            cout << a[i].name << " - " << a[i].phone << "\n";
            count++;
        }
    }
}

int main() {
    Contact a[] = {
        {"Nguyen Van Minh", "0901234567"},
        {"Tran Thi Minh Anh", "0912345678"},
        {"Le Minh Tuan", "0923456789"},
        {"Pham Van Nam", "0931111111"},
        {"Hoang Minh Duc", "0942222222"}
    };

    int n = 5;

    string key;
    cout << "Nhap ten can tim: ";
    getline(cin, key);

    int steps;
    searchName(a, n, key, steps);

    suggest(a, n, key);

    sortPhone(a, n);

    cout << "\nTim theo SĐT (Binary Search)\n";
    string phone;
    cin >> phone;

    int pos = binaryPhone(a, n, phone);

    if (pos != -1)
        cout << "Found: " << a[pos].name;
    else
        cout << "Not found";

    cout << "\nThoi gian thuc thi rat nho (~micro giay)\n";
}
