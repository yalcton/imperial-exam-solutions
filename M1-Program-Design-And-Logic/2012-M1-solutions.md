### 2 bi)

```
∀ M,A [(m(M) ^ a(A) ^ em(M,A)] ^ 
∃D [d(D) ^ ∀M(em(D,M))]
```

### 2 bii)

```
∀ C [c(C) ^ ¬sc(C) --> ∃M1, M2 (m(M1) ^ m(M2) ^ sign(C,M1) ^ sign(C,M2) ^ ¬(M1=M2) 
                       ^ ¬∃X(sign(C,X) ^ ¬(X=M1) ^ ¬(X=M2))] ^
∀ C [c(C) ^ sc(C) --> ∃D(sign(C,D) ^ d(D) ^ ¬∃X(sign(C,X) ^ ¬(X=D)]
```

### 2 biii)

```
∀ D, C ( d(D) ^ c(C) ^ sign(D,C) --> ∃M(m(M) ^ app(D,M,C)) v
∀ D, C ( d(D) ^ c(C) ^ sign(D,C) --> ∃A, ∀C(a(A) ^ worksFor(A,D) ^ delegates(D,A,C) ^ sign(D,C))
```
### 3 a)

```
class Animal {
  string name;
  int x1, y1, x2, y2;
  public:
  string get_name();
  bool in_area(int, int);
  Animal (string, int, int, int, int);
}

class Tag{
  int id;
  int battery;
  int x1, y1;
  public:
  void transmit(Animal *, int, int, int);
}
```
### 3 b)

```
int main(){
  Table <Tag*, Animal*> myTable;
  Animal* lassie = new Animal("Lassie", 0, 0, 5, 5);
  Tag* 111 = new Tag(111);
  myTable.set(111, lassie);
  
  Animal* garfield = new Animal("Garfield", 3, 4, 7, 8);
  Tag* 222 = new Tag(222);
  myTable.set(222, garfield);
  
  111->transmit(myTable.get(111), 3, 3, 15);
  222->transmit(myTable.get(222), 8, 8, 50);
}
```
### 3 c)
```
Animal::Animal(string a, int _x1, int _y1, int _x2, int _y2)
          :name(a),x1(_x1),y1(_y1),x2(_x2),y2(_y2){}
          
string Animal::get_name(){
  return name;
}

bool Animal::in_area(int x, int y){
  if(x<x1 || x>x2 || y<y1 || y>y2) //assumed that x1,x2,y1,y2 are given in ascending order
      return false;
  return true;
}

Tag::Tag(int a):id(a){
  battery = 100;
}

void Tag::transmit(Animal* a, int x, int y, int b){
  battery = b;
  if(!a->in_area(x, y))
    cout<<"Animal "<<a->get_name()<<" is outside the area";
  if(battery < 20)
    cout<<"Battery is low";
}

```
### Question3 Alternative
```
class Tag {
  private: 
    int number; string name; System *system;
  public:
    Tag(int, string, System&);
    void transmit(int, int, int);
};
class Area {
  private:
    int x_low, y_low, x_high, y_high;
  public: 
    Area(int, int, int, int);
};
class System {
  private: 
    Table<Tag,Area> tag_areas;
  public:
    System(Table<Tag,Area>);
    void receive(Readign& r, Tag& t);
};
class Reading {
  private:
    int x_position, y_position, battery;
  public:
    Reading(int, int ,int);
};

//============================================

Table tag_areas();
System *system = new System(tag_areas);

Area* l_area = new Area(0,0,5,5);
Tag* lassie = new Tag(111, "Lassie", system);

system -> tag_areas.set(lassie, l_area);

lassie.transmit(3,3,15);

//============================================

Area::Area (int xl, int yl, int xh, int yh) {
  x_low = xl; x_high = xh; y_low = yl, y_high = yh;
}
Tag::Tag(int _number, int _name, System &s) {
  number = _number; name = _name; system = s;
}
System::System(Table _tag_table) {
  tag_table = _tag_table;
}
Reading::Reading(int x, int y, int _battery) {
  x_position = x; y_position = y; battery = _battery;
}

void Tag::Transmit(int x_pos, int y_pos, int battery) {
  Reading * reading = new Reading(x_pos, y_pos, battery);
  system->receive(reading, this);
}

void System::receive (Reading &r, Tag& t) {
  Area * area_to_check = tag_areas.get(t);
  if (r->x_pos < t->x_low || r->x_pos > t->x_high .... )
    cout << t->name << " out of area";
  if (r->battery < 20)
    cout << t->name << "'s battery is low";
}
```


### 4 b)
```
class Battery{
  double max, available, rechargeP;
  
  public:
  Battery(double,double);
  void use(double);
  void recharge (double);
  double get_charge();
}

class Device{
  Battery * battery;
  
  public:
  Device(Battery*);
  virtual void operate(double) = 0;
  void recharge(double);
  double ant_life();
}

class Phone: public Device{
  int cores, tasks;
  
  public:
  Phone(Battery*, int);
  virtual void operate(double);
  void set_tasks(int);
}

class Camera: public Device{
  int consumption;
  
  public:
  Camera(Battery*);
  virtual void operate(double);
}
```

### 4 c)

```
int main(){
  Battery * b1 = new Battery(6000,500);
  Device * Canon = new Camera(b1);
  Canon->operate(1.5);
  Canon->recharge(0.5);
  cout<<Canon->ant_life();
  Battery * b2 = new Battery(4000,800);
  Device * Samsung = new Phone(b2, 2);
  Samsung->set_tasks(3);
  Samsung->operate(2);
  cout<<Samsung->ant_life();
}
```
### 4 d)

```
Battery::Battery(double a, double b):max(a),rechargeP(b){
  available = max;
}
void Battery::use(double a){ //a here is amount consumed
  if (a>available)
    available = 0;
  else
    available -= a;
}
void Battery::recharge(double t){
  if (t*rechargeP + available > max)
    available = max;
  else
    available += t*rechargeP;
}
double Battery::get_charge(){
  return available;
}

Device::Device(Battery* b): battery(b){}
void Device::recharge(double t){
  this.battery->recharge(t);
}
double Device::ant_life(){
  return battery->get_charge();
}

Phone::Phone(Battery* b, int a):Device(b),cores(a){
  tasks = 0;
}
void Phone::set_tasks(int a){
  tasks = a;
}

void Phone::operate(double t){
  if (cores>tasks)
    battery->use(tasks*t*500 + 500);
  else
    battery->use(cores*t*500 + 500);
}
Camera::Camera(Battery * b):Device(b){
  consumption = 1500;
}
void Camera::operate(double t){
  battery->use(t*consumption);
}
  
```
