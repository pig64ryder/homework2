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
