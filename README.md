# cab-service
A cab booking system prototype designed around women's safety — built as a learning project exploring booking flows and verification logic.
import datetime

class Passenger:
    def __init__(self, name, phone_number):
        self.name = name
        self.phone_number = phone_number

class Driver:
    def __init__(self, name, phone_number, vehicle_number):
        self.name = name
        self.phone_number = phone_number
        self.vehicle_number = vehicle_number

class CabService:
    def __init__(self):
        self.passengers = []
        self.drivers = []
        self.ride_requests = []

    def add_passenger(self, name, phone_number):
        passenger = Passenger(name, phone_number)
        self.passengers.append(passenger)
        print(f"Passenger {name} added successfully!")

    def add_driver(self, name, phone_number, vehicle_number):
        driver = Driver(name, phone_number, vehicle_number)
        self.drivers.append(driver)
        print(f"Driver {name} added successfully!")

    def book_ride(self, passenger_name, pickup_location, drop_off_location):
        passenger = next((p for p in self.passengers if p.name == passenger_name), None)
        if passenger:
            ride_request = {
                "passenger_name": passenger_name,
                "pickup_location": pickup_location,
                "drop_off_location": drop_off_location,
                "status": "pending"
            }
            self.ride_requests.append(ride_request)
            print(f"Ride booked successfully for {passenger_name}!")
        else:
            print("Passenger not found!")

    def assign_driver(self, ride_request):
        driver = next((d for d in self.drivers if d.vehicle_number == "available"), None)
        if driver:
            ride_request["driver_name"] = driver.name
            ride_request["driver_phone_number"] = driver.phone_number
            ride_request["status"] = "assigned"
            print(f"Driver {driver.name} assigned to ride request!")
        else:
            print("No drivers available!")

    def update_ride_status(self, ride_request, status):
        ride_request["status"] = status
        print(f"Ride status updated to {status}!")

    def display_ride_requests(self):
        for ride_request in self.ride_requests:
            print(f"Passenger: {ride_request['passenger_name']}")
            print(f"Pickup Location: {ride_request['pickup_location']}")
            print(f"Drop-off Location: {ride_request['drop_off_location']}")
            print(f"Driver: {ride_request.get('driver_name', 'Not assigned')}")
            print(f"Status: {ride_request['status']}")
            print("------------------------")

def main():
    cab_service = CabService()

    while True:
        print("1. Add Passenger")
        print("2. Add Driver")
        print("3. Book Ride")
        print("4. Assign Driver")
        print("5. Update Ride Status")
        print("6. Display Ride Requests")
        print("7. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            name = input("Enter passenger name: ")
            phone_number = input("Enter passenger phone number: ")
            cab_service.add_passenger(name, phone_number)
        elif choice == "2":
            name = input("Enter driver name: ")
            phone_number = input("Enter driver phone number: ")
            vehicle_number = input("Enter vehicle number: ")
            cab_service.add_driver(name, phone_number, vehicle_number)
        elif choice == "3":
            passenger_name = input("Enter passenger name: ")
            pickup_location = input("Enter pickup location: ")
            drop_off_location = input("Enter drop-off location: ")
            cab_service.book_ride(passenger_name, pickup_location, drop_off_location)
        elif choice == "4":
            ride_request = cab_service.ride_requests[0]
            cab_service.assign_driver(ride_request)
        elif choice == "5":
            ride_request = cab_service.ride_requests[0]
            status = input("Enter new ride status: ")
            cab_service.update_ride_status(ride_request, status)
        elif choice == "6":
            cab_service.display_ride_requests()
        elif choice == "7":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
