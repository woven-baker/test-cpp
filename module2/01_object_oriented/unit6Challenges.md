# Challenges

## Challenge 1
Create a scoped enum `TrafficLight` representing the colors of a traffic light: `red`, `yellow`, and `green`.

Complete the following function that takes a `TrafficLight` as an input and returns the action corresponding to the traffic light color as a string.

```cpp
std::string get_action(TrafficLight light) {
    // TODO
}
```

Input:

```cpp
TrafficLight::red
```

Output: 

```cpp
"Stop"
```

---

```cpp
#include <iostream>
#include <string>

enum class TrafficLight {
    red,
    yellow,
    green
};

std::string get_action(TrafficLight light) {
    switch (light) {
        case TrafficLight::Red: return "Stop";
        case TrafficLight::Yellow: return "Caution";
        case TrafficLight::Green: return "Go";
        default: return "Invalid traffic light color";
    }
}

int main() {
    TrafficLight light = TrafficLight::Red;
    std::cout << get_action(light) << std::endl;
    return 0;
}
```

## Challenge 2

Define a struct `Rectangle` with two float members: `width` and `height`. Write a function `rectangle_area` that takes a `Rectangle` object as input and returns its area.

Input:

```
4 5
```

Output:
```
20
```

---

```cpp
#include <iostream>

struct Rectangle {
    float width;
    float height;
};

constexpr double rectangle_area(const Rectangle& rect) {
    return rect.width * rect.height;
}

int main() {
    Rectangle rect{3.0f, 4.0f};
    std::cout << "Area: " << rectangle_area(rect) << std::endl;
    return 0;
}
```

## Challenge 3
Create a `BankAccount` class with the following features:

- A private member variable `balance` (type double) to store the account balance.
- A constructor that takes an initial balance as a parameter.
- A member function `deposit` (type double) to deposit the given amount into the account.
- A member function `withdraw` (type double) to withdraw the given amount from the account. If there are insufficient funds, print "Insufficient balance!" and do not perform the withdrawal.
- A member function `get_balance` to return the current balance.

Implement the `BankAccount` class and use it to perform a series of transactions.

Input:

```cpp
BankAccount bank_account(100);
bank_account.deposit(50);
bank_account.withdraw(30);
bank_account.withdraw(150);
```

Output:

```
120
90
Insufficient balance!
```

---

```cpp
#include <iostream>

class BankAccount {
public:
    BankAccount(double initial_balance) : balance(initial_balance) {}

    void deposit(double amount) {
        balance += amount;
        std::cout << balance << std::endl;
    }

    void withdraw(double amount) {
        if (amount > balance) {
            std::cout << "Insufficient balance!" << std::endl;
        } else {
            balance -= amount;
            std::cout << balance << std::endl;
        }
    }

    double get_balance() const {
        return balance;
    }

private:
    double balance;
};

int main() {
    BankAccount bank_account(100);
    bank_account.deposit(50);
    bank_account.withdraw(30);
    bank_account.withdraw(150);
    return 0;
}
```

## Challenge 4
Implement a `Person` class with a `std::string` name member variable and a constructor that takes a name as a parameter.

Implement a `Team` class with the following features:

- A `std::vector<Person>` member variable representing the team members.
- A member function `add_member` to add a member to the team.

Implement a `Project` class with the following features:

- A private `Team` member variable representing the project team.
- A constructor that initializes the `Team` object.
- A member function `add_team_member` to add a member to the team.
- A member function `print_team_members` to print the members of the team.

---

```cpp
#include <iostream>
#include <string>
#include <vector>

class Person {
public:
    Person(const std::string& name) : name(name) {}

    std::string get_name() const {
        return name;
    }

private:
    std::string name;
};

class Team {
public:
    void add_member(const Person& member) {
        members.push_back(member);
    }

    void print_members() const {
        for (const Person& member : members) {
            std::cout << member.get_name() << std::endl;
        }
    }

private:
    std::vector<Person> members;
};

class Project {
public:
    Project() : team() {}

    void add_team_member(const Person& member) {
        team.add_member(member);
    }

    void print_team_members() const {
        team.print_members();
    }

private:
    Team team;
};

int main() {
    Project project;
    project.add_team_member(Person("Alice"));
    project.add_team_member(Person("Bob"));
    project.add_team_member(Person("Charlie"));

    std::cout << "Project team members:" << std::endl;
    project.print_team_members();

    return 0;
}
```

## Challenge 5

Refactor the previous challenge's code to instead support adding projects to a team. A team can have multiple projects, and for each project there is a member of the team that has been selected as the lead member of the project. Be careful about data duplication and use pointers (or smart pointers) where necessary.

---

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <memory>

class Person {
public:
    Person(const std::string& name) : name(name) {}

    std::string get_name() const {
        return name;
    }

private:
    std::string name;
};

class Project {
public:
    Project(const std::string& project_name, const std::shared_ptr<Person>& lead_member)
        : project_name(project_name), lead_member(lead_member) {}

    std::string get_project_name() const {
        return project_name;
    }

    std::string get_lead_member_name() const {
        return lead_member->get_name();
    }

private:
    std::string project_name;
    std::shared_ptr<Person> lead_member;
};

class Team {
public:
    void add_member(const std::shared_ptr<Person>& member) {
        members.push_back(member);
    }

    void add_project(const Project& project) {
        projects.push_back(project);
    }

    void print_members() const {
        std::cout << "Team members:" << std::endl;
        for (const auto& member : members) {
            std::cout << member->get_name() << std::endl;
        }
    }

    void print_projects() const {
        std::cout << "Projects:" << std::endl;
        for (const Project& project : projects) {
            std::cout << "Project Name: " << project.get_project_name()
                      << ", Lead Member: " << project.get_lead_member_name() << std::endl;
        }
    }

private:
    std::vector<std::shared_ptr<Person>> members;
    std::vector<Project> projects;
};

int main() {
    Team team;

    auto alice = std::make_shared<Person>("Alice");
    auto bob = std::make_shared<Person>("Bob");
    auto charlie = std::make_shared<Person>("Charlie");

    team.add_member(alice);
    team.add_member(bob);
    team.add_member(charlie);

    Project project1("Project 1", alice);
    Project project2("Project 2", bob);
    Project project3("Project 3", charlie);

    team.add_project(project1);
    team.add_project(project2);
    team.add_project(project3);

    team.print_members();
    std::cout << std::endl;
    team.print_projects();

    return 0;
}
```