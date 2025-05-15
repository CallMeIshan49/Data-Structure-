#include <iostream>
#include <vector>
#include <string>
#include <utility> // for std::pair

using namespace std;

class Route {
public:
    int id;
    string name;
    vector<string> stops;

    Route(int id, string name) : id(id), name(name) {}

    void add_stop(string stop) {
        stops.push_back(stop);
    }

    void display() const {
        cout << "Route ID: " << id << ", Name: " << name << ", Stops: ";
        for (const auto& stop : stops) {
            cout << stop << " ";
        }
        cout << endl;
    }
};

class Vehicle {
public:
    int id;
    string type;
    int capacity;
    string location;

    Vehicle(int id, string type, int capacity) : id(id), type(type), capacity(capacity), location("Garage") {}

    void update_capacity(int new_capacity) {
        capacity = new_capacity;
    }

    void update_location(string new_location) {
        location = new_location;
    }

    void display() const {
        cout << "Vehicle ID: " << id << ", Type: " << type << ", Capacity: " << capacity << ", Location: " << location << endl;
    }
};

class Schedule {
public:
    int vehicle_id;
    int route_id;
    string time;

    Schedule(int vehicle_id, int route_id, string time) : vehicle_id(vehicle_id), route_id(route_id), time(time) {}

    void update_time(string new_time) {
        time = new_time;
    }

    void display() const {
        cout << "Schedule - Vehicle ID: " << vehicle_id << ", Route ID: " << route_id << ", Time: " << time << endl;
    }
};

class User {
public:
    int id;
    string name;
    vector<pair<int, int>> booked_tickets; // Pair of (Vehicle ID, Route ID)

    User(int id, string name) : id(id), name(name) {}

    void book_ticket(int vehicle_id, int route_id) {
        booked_tickets.push_back(make_pair(vehicle_id, route_id));
    }

    void display() const {
        cout << "User ID: " << id << ", Name: " << name << ", Booked Tickets: ";
        for (const auto& ticket : booked_tickets) {
            cout << "(Vehicle ID: " << ticket.first << ", Route ID: " << ticket.second << ") ";
        }
        cout << endl;
    }
};

class TransportSystem {
private:
    vector<Route> routes;
    vector<Vehicle> vehicles;
    vector<Schedule> schedules;
    vector<User> users;

public:
    void add_route(const Route& route) {
        routes.push_back(route);
    }

    void add_vehicle(const Vehicle& vehicle) {
        vehicles.push_back(vehicle);
    }

    void add_schedule(const Schedule& schedule) {
        schedules.push_back(schedule);
    }

    void add_user(const User& user) {
        users.push_back(user);
    }

    void book_ticket(int user_id, int vehicle_id, int route_id) {
        User* user = find_user_by_id(user_id);
        if (user) {
            user->book_ticket(vehicle_id, route_id);
            cout << "Ticket booked successfully for User ID: " << user_id << endl;
        } else {
            cout << "User not found!" << endl;
        }
    }

    void update_vehicle_location(int vehicle_id, string location) {
        Vehicle* vehicle = find_vehicle_by_id(vehicle_id);
        if (vehicle) {
            vehicle->update_location(location);
            cout << "Vehicle location updated to: " << location << endl;
        } else {
            cout << "Vehicle not found!" << endl;
        }
    }

    void generate_report() const {
        cout << "=== Transport System Report ===" << endl;
        for (const auto& route : routes) {
            route.display();
        }
        for (const auto& vehicle : vehicles) {
            vehicle.display();
        }
        for (const auto& schedule : schedules) {
            schedule.display();
        }
        for (const auto& user : users) {
            user.display();
        }
    }

    Route* find_route_by_id(int route_id) {
        for (auto& route : routes) {
            if (route.id == route_id) {
                return &route;
            }
        }
        return nullptr;
    }

    Vehicle* find_vehicle_by_id(int vehicle_id) {
        for (auto& vehicle : vehicles) {
            if (vehicle.id == vehicle_id) {
                return &vehicle;
            }
        }
        return nullptr;
    }

    User* find_user_by_id(int user_id) {
        for (auto& user : users) {
            if (user.id == user_id) {
                return &user;
            }
        }
        return nullptr;
    }

    void display() const {
        cout << "Transport System: " << endl;
        for (const auto& route : routes) {
            route.display();
        }
        for (const auto& vehicle : vehicles) {
            vehicle.display();
        }
        for (const auto& schedule : schedules) {
            schedule.display();
        }
        for (const auto& user : users) {
            user.display();
        }
    }
};

void input_route(TransportSystem& ts) {
    int id;
    string name;
    cout << "Enter Route ID: ";
    cin >> id;
    cout << "Enter Route Name: ";
    cin.ignore();
    getline(cin, name);
    Route route(id, name);

    int num_stops;
    cout << "Enter number of stops: ";
    cin >> num_stops;
    cin.ignore();

    for (int i = 0; i < num_stops; ++i) {
        string stop;
        cout << "Enter stop " << i + 1 << ": ";
        getline(cin, stop);
        route.add_stop(stop);
    }

    ts.add_route(route);
}

void input_vehicle(TransportSystem& ts) {
    int id;
    string type;
    int capacity;
    cout << "Enter Vehicle ID: ";
    cin >> id;
    cout << "Enter Vehicle Type: ";
    cin.ignore();
    getline(cin, type);
    cout << "Enter Vehicle Capacity: ";
    cin >> capacity;
    cin.ignore();
    Vehicle vehicle(id, type, capacity);
    ts.add_vehicle(vehicle);
}

void input_schedule(TransportSystem& ts) {
    int vehicle_id, route_id;
    string time;
    cout << "Enter Vehicle ID: ";
    cin >> vehicle_id;
    cout << "Enter Route ID: ";
    cin >> route_id;
    cout << "Enter Time (e.g., 09:00 AM): ";
    cin.ignore();
    getline(cin, time);
    Schedule schedule(vehicle_id, route_id, time);
    ts.add_schedule(schedule);
}

void input_user(TransportSystem& ts) {
    int id;
    string name;
    cout << "Enter User ID: ";
    cin >> id;
    cout << "Enter User Name: ";
    cin.ignore();
    getline(cin, name);
    User user(id, name);
    ts.add_user(user);
}

int main() {
    TransportSystem ts;
    int choice;

    do {
        cout << "\nTransport System Menu:\n";
        cout << "1. Add Route\n";
        cout << "2. Add Vehicle\n";
        cout << "3. Add Schedule\n";
        cout << "4. Add User\n";
        cout << "5. Book Ticket\n";
        cout << "6. Update Vehicle Location\n";
        cout << "7. Generate Report\n";
        cout << "8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            input_route(ts);
            break;
        case 2:
            input_vehicle(ts);
            break;
        case 3:
            input_schedule(ts);
            break;
        case 4:
            input_user(ts);
            break;
        case 5: {
            int user_id, vehicle_id, route_id;
            cout << "Enter User ID: ";
            cin >> user_id;
            cout << "Enter Vehicle ID: ";
            cin >> vehicle_id;
            cout << "Enter Route ID: ";
            cin >> route_id;
            ts.book_ticket(user_id, vehicle_id, route_id);
            break;
        }
        case 6: {
            int vehicle_id;
            string location;
            cout << "Enter Vehicle ID: ";
            cin >> vehicle_id;
            cout << "Enter new Location: ";
            cin.ignore();
            getline(cin, location);
            ts.update_vehicle_location(vehicle_id, location);
            break;
        }
        case 7:
            ts.generate_report();
            break;
        case 8:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 8);

    return 0;
}
