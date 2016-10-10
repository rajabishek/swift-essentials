class Employee:
    empCount = 0

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1

    def displayCount(self):
        print("Total Employee {}".format(Employee.empCount))

    def displayEmployee(self):
        print("Name: {}, Salary: {}".format(self.name, self.salary))

rajabishek = Employee('Raj Abishek', 12000)
rajabishek.displayEmployee()
