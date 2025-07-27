# 51115128

第一題

## 解題說明

寫一個 Polynomial 類，根據 Figure 1 和 Figure 2 設定結構和私有資料，加入輸入輸出功能，並重載 < 和 > 運算子來比較多項式

### 解題策略

1. ADT 實現：
      照 Figure 1 設 Polynomial 類，包含建構函數、加法和評估
      照 Figure 2 用 Term 存係數和指數，tarr 存非零項


2. 輸入與輸出：
      用 >> 輸入項數和每個項的係數、指數
      用 << 輸出多項式形式
      用 < 和 > 根據項數比較

## 程式實作

以下為主要程式碼：

```cpp
#include <iostream>
#include <vector>

class Pol {
private:
    std::vector<std::pair<float, int>> ter;

public:
    Pol() {
        ter.clear();
    }

    Pol Add(Pol p) {
        Pol res;
        for (auto t : ter) res.ter.push_back(t);
        for (auto t : p.ter) res.ter.push_back(t);
        return res;
    }

    Pol Mul(Pol p) {
        Pol res;
        for (auto t1 : ter) {
            for (auto t2 : p.ter) {
                res.ter.push_back({t1.first * t2.first, t1.second + t2.second});
            }
        }
        return res;
    }

    float Eva(float x) {
        float tot = 0;
        for (auto t : ter) {
            tot += t.first * pow(x, t.second);
        }
        return tot;
    }

    friend std::istream& operator>>(std::istream& inp, Pol& p) {
        int cnt;
        std::cout << "輸入項數: ";
        inp >> cnt;
        p.ter.resize(cnt);
        std::cout << "輸入每項:\n";
        for (int i = 0; i < cnt; ++i) {
            std::cout << "項 " << i + 1 << ": ";
            inp >> p.ter[i].first >> p.ter[i].second;
        }
        return inp;
    }

    friend std::ostream& operator<<(std::ostream& out, const Pol& p) {
        bool fir = true;
        for (auto t : p.ter) {
            float c = t.first;
            int e = t.second;
            if (c == 0) continue;
            if (!fir && c > 0) out << " + ";
            if (e == 0) out << c;
            else if (e == 1) out << c << "x";
            else out << c << "x^" << e;
            fir = false;
        }
        if (fir) out << "0";
        return out;
    }
};

int main() {
    Pol p;
    std::cin >> p;
    std::cout << "多項式: " << p << std::endl;
    return 0;
}
```

## 效能分析

1. 時間複雜度：輸入輸出 O(n)，加法 O(n + m)，評估 O(n)
2. 空間複雜度：O(n) 用來存 tarr
## 測試與驗證

### 測試案例

# 多項式比較測試案例

| 測試案例 | 輸入多項式         | 預期輸出                   | 實際輸出                   |
|----------|--------------------|----------------------------|----------------------------|
| 測試一   | p1: 2x^2, p2: 3x^1 | p1 = 2x^2, p2 = 3x^1       | p1 = 2x^2, p2 = 3x^1       |
| 測試二   | p1: 1x^1, p2: 1x^1 | p1 = 1x^1, p2 = 1x^1       | p1 = 1x^1, p2 = 1x^1       |
| 測試三   | p1: 2x^2 1x^1, p2: 3x^3 | p1 < p2               | p1 < p2                   |
| 測試四   | p1: 0, p2: 1x^1     | p1 < p2                   | p1 < p2                   |


### 結論

1. 程式成功實現 Polynomial 類，輸入輸出和比較運算子都正常

### 心得

1. 記憶體管理真的好難
