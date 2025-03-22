// Объект- банковский
кредит (поля:
название, сумма
кредита, тип
валюты, ставка в %
годовых)
Сортировка по
процентам//
#include <iostream>
#include <fstream>
#include <vector>
#include <deque>
#include <set>
#include <unordered_set>
#include <string>

using namespace std;

class Credit {
private:
    string Name;
    int Sumkr;
    string Type;
    double Stavka;

public:
    Credit() {
        Name = "";
        Sumkr = 0;
        Type = "";
        Stavka = 0.0;
    }

    Credit(string Name, int Sumkr, string Type, double Stavka) {
        this->Name = Name;
        this->Sumkr = Sumkr;
        this->Type = Type;
        this->Stavka = Stavka;
    }

    string getName() const {
        return this->Name;
    }

    string getType() const {
        return this->Type;
    }

    int sumKr() const {
        return this->Sumkr;
    }

    double getStavka() const {
        return Stavka;
    }

    Credit operator=(const Credit& _credit) {
        this->Name = _credit.Name;
        this->Sumkr = _credit.Sumkr;
        this->Type = _credit.Type;
        this->Stavka = _credit.Stavka;
        return *this;
    }

    void print(ostream& os) const {
        os << "Name= " << Name << " Sumkr= " << Sumkr << " Type=" << Type << " Stavka=" << Stavka << endl;
    }
    bool operator<(const Credit& other) const {
        return this->Stavka < other.Stavka;
    }
    bool operator==(const Credit& other) const {
        return this->Name == other.Name && this->Sumkr == other.Sumkr && this->Type == other.Type && this->Stavka == other.Stavka;
    }
};
struct CreditHash {
    size_t operator()(const Credit& credit) const {
        return hash<string>()(credit.getName()) ^ hash<int>()(credit.sumKr()) ^ hash<string>()(credit.getType()) ^ hash<double>()(credit.getStavka());
    }
};

int main() {
    ifstream fs;
    int n;
    fs.open("input.txt");
    if (!fs.is_open()) {
        cout << "Error" << endl;
        return 1;
    }
    fs >> n;
    string Na, Ty;
    int Su;
    double St;
    vector<Credit> vec;
    while (!fs.eof()) {
        fs >> Na >> Su >> Ty >> St;
        vec.push_back(Credit(Na, Su, Ty, St));
    }
    set<Credit> creditSet;
    for (const auto& credit : vec) {
        creditSet.insert(credit);
    }
    unordered_set<Credit, CreditHash> creditUnorderedSet;
    for (const auto& credit : vec) {
        creditUnorderedSet.insert(credit);
    }
    cout << "Set" << endl;
    for (const auto& credit : creditSet) {
        credit.print(cout);
    }
    cout << "Unordered_set" << endl;
    for (const auto& credit : creditUnorderedSet) {
        credit.print(cout);
    }

    return 0;
}
