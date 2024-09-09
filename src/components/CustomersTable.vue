<template>
    <div class="row mb-4" v-if="!showDetails">
        <div class="col-12">
            <div class="card shadow-sm">
                <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
                    <!-- Header Title -->
                    <h5 class="m-0">Customer Management</h5>
                    <div>
                        <!-- Generate Report Dropdown -->
                        <div class="dropdown d-inline me-2">
                            <button class="btn btn-sm btn-success dropdown-toggle" type="button"
                                id="generateReportDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                                Generate Report
                            </button>
                            <ul class="dropdown-menu" aria-labelledby="generateReportDropdown">
                                <li><a class="dropdown-item" href="#" @click="generateReport('Excel/CSV')">Excel/CSV</a>
                                </li>
                                <li><a class="dropdown-item" href="#" @click="generateReport('PDF')">PDF</a></li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="card-body">
                    <!-- Search and Toggle Filters Controls -->
                    <div class="row mb-3">
                        <div class="col-md-9">
                            <input type="text" v-model="searchQuery" class="form-control"
                                placeholder="Search by ID, Username, Shipping Address or Phone Number">
                        </div>
                        <div class="col-md-3">
                            <button class="btn btn-secondary w-100" @click="toggleFilters">
                                Filter Customers
                            </button>
                        </div>
                    </div>

                    <!-- Filters Row (Toggle Visibility) -->
                    <div v-if="filterVisibility" class="row mb-3">
                        <div class="col-md-4">
                            <label for="filterGender">Filter by Gender:</label>
                            <select v-model="filterGender" class="form-control">
                                <option value="">All Genders</option>
                                <option value="Male">Male</option>
                                <option value="Female">Female</option>
                                <option value="Other">Other</option>
                            </select>
                        </div>
                        <div class="col-md-4">
                            <label for="filterDOB">Filter by Date of Birth:</label>
                            <input type="date" v-model="filterDOB" class="form-control">
                        </div>
                        <div class="col-md-4">
                            <label for="filterDate">Created Date:</label>
                            <input type="date" v-model="filterDate" class="form-control">
                        </div>
                    </div>

                    <!-- Table -->
                    <table class="table table-striped table-bordered w-100">
                        <thead class="thead-dark">
                            <tr>
                                <th>ID</th>
                                <th>Username</th>
                                <th>Email</th>
                                <th>Shipping Address</th>
                                <th>Created Date</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="customer in filteredCustomers" :key="customer.id">
                                <td>{{ customer.id }}</td>
                                <td>{{ customer.username }}</td>
                                <td>{{ customer.email }}</td>
                                <td>
                                    <ul style="list-style-type: square; padding-left: 1rem; margin-bottom: 0;">
                                        <li v-for="address in customer.shippingAddresses" :key="address">{{ address }}
                                        </li>
                                    </ul>
                                </td>
                                <td>{{ formatCreatedTime(customer.createdTime) }}</td>
                                <td>
                                    <button class="btn btn-sm btn-warning me-2" @click="viewDetails(customer)">View
                                        Details</button>
                                    <button class="btn btn-sm btn-primary me-2"
                                        @click="editCustomer(customer)">Edit</button>
                                    <button class="btn btn-sm btn-danger"
                                        @click="deleteCustomer(customer.id)">Delete</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Use CustomersDetails component for details view -->
    <CustomersDetails v-else :customer="selectedCustomer" @back-to-table="backToTable" />
</template>


<script>
import CustomersDetails from './CustomersDetails.vue';
import jsPDF from "jspdf"; // Import jsPDF for generating PDF files
import "jspdf-autotable"; // Import AutoTable plugin for jsPDF

export default {
    name: "CustomersTable",
    components: {
        CustomersDetails,
    },
    data() {
        return {
            searchQuery: "", // Search query for filtering customers
            filterGender: "", // Filter for gender
            filterDOB: "", // Filter for date of birth
            filterDate: "", // Filter for created date
            filterVisibility: false, // Control the visibility of filters
            editCustomerId: null, // ID of the customer being edited
            editCustomerData: {}, // Temporary data for the customer being edited
            showDetails: false, // Control the visibility of the details view
            selectedCustomer: null, // Store selected customer for details view
            customers: [
                { id: 1, username: 'johndoe', name: 'John Doe', email: 'john@example.com', phoneNumber: '123-456-7890', gender: 'Male', dob: '1990-01-01', shippingAddresses: ['123 Main St', '456 Oak St'], createdTime: '2024-09-01T19:00:00' },
                { id: 2, username: 'janesmith', name: 'Jane Smith', email: 'jane@example.com', phoneNumber: '234-567-8901', gender: 'Female', dob: '1985-02-14', shippingAddresses: ['789 Pine St'], createdTime: '2024-09-01T13:30:00' },
                { id: 3, username: 'michaeljohnson', name: 'Michael Johnson', email: 'michaelj@example.com', phoneNumber: '345-678-9012', gender: 'Male', dob: '1992-03-20', shippingAddresses: ['101 Maple Ave', '202 Cedar Ave'], createdTime: '2024-08-25T10:00:00' },
                { id: 4, username: 'emilydavis', name: 'Emily Davis', email: 'emilyd@example.com', phoneNumber: '456-789-0123', gender: 'Female', dob: '1988-07-08', shippingAddresses: ['303 Birch Ln'], createdTime: '2024-08-30T14:15:00' },
                { id: 5, username: 'williambrown', name: 'William Brown', email: 'williamb@example.com', phoneNumber: '567-890-1234', gender: 'Male', dob: '1975-11-30', shippingAddresses: ['404 Elm St', '505 Spruce St'], createdTime: '2024-09-02T08:45:00' },
                { id: 6, username: 'lindamiller', name: 'Linda Miller', email: 'lindam@example.com', phoneNumber: '678-901-2345', gender: 'Female', dob: '1981-05-15', shippingAddresses: ['606 Fir Dr'], createdTime: '2024-09-03T11:20:00' },
                { id: 7, username: 'jameswilson', name: 'James Wilson', email: 'jamesw@example.com', phoneNumber: '789-012-3456', gender: 'Male', dob: '1995-09-09', shippingAddresses: ['707 Oakwood Blvd', '808 Pinehurst Dr'], createdTime: '2024-09-04T16:00:00' },
                { id: 8, username: 'patriciamoore', name: 'Patricia Moore', email: 'patriciam@example.com', phoneNumber: '890-123-4567', gender: 'Female', dob: '1998-04-22', shippingAddresses: ['909 Willow Rd'], createdTime: '2024-08-28T09:10:00' },
                { id: 9, username: 'roberttaylor', name: 'Robert Taylor', email: 'robertt@example.com', phoneNumber: '901-234-5678', gender: 'Male', dob: '1967-12-05', shippingAddresses: ['1010 Poplar St', '1111 Hickory St'], createdTime: '2024-09-01T18:30:00' },
                { id: 10, username: 'barbaraanderson', name: 'Barbara Anderson', email: 'barbaraa@example.com', phoneNumber: '012-345-6789', gender: 'Female', dob: '1979-10-18', shippingAddresses: ['1212 Ash Dr'], createdTime: '2024-08-29T15:55:00' },
                { id: 11, username: 'davidthomas', name: 'David Thomas', email: 'davidt@example.com', phoneNumber: '123-456-7891', gender: 'Male', dob: '1972-08-01', shippingAddresses: ['1313 Cherry Ln', '1414 Chestnut Ave', '1515 Walnut Blvd'], createdTime: '2024-08-27T12:05:00' }
            ]
        };
    },
    computed: {
        filteredCustomers() {
            let filtered = this.customers;

            // Apply search filter by ID, username, or phone number
            if (this.searchQuery) {
                filtered = filtered.filter(customer =>
                    customer.id.toString().includes(this.searchQuery) ||
                    customer.username.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
                    customer.phoneNumber.includes(this.searchQuery) ||
                    customer.shippingAddresses.some(address => address.toLowerCase().includes(this.searchQuery.toLowerCase()))
                );
            }

            // Apply gender filter
            if (this.filterGender) {
                filtered = filtered.filter(customer => customer.gender === this.filterGender);
            }

            // Apply filter by date of birth
            if (this.filterDOB) {
                filtered = filtered.filter(customer => new Date(customer.dob).toISOString().slice(0, 10) === this.filterDOB);
            }

            // Apply filter by created time
            if (this.filterDate) {
                filtered = filtered.filter(customer => new Date(customer.createdTime).toLocaleDateString() === new Date(this.filterDate).toLocaleDateString());
            }

            return filtered;
        }
    },
    methods: {
        toggleFilters() {
            this.filterVisibility = !this.filterVisibility; // Toggle filter row visibility
        },
        viewDetails(customer) {
            this.selectedCustomer = customer;
            this.showDetails = true; // Show details view
        },
        backToTable() {
            this.showDetails = false; // Back to table view
        },
        generateReport(type) {
            const dataToExport = this.filteredCustomers; // Use filtered data for export
            if (type === "Excel/CSV") {
                this.exportToCSV(dataToExport);
            } else if (type === "PDF") {
                this.exportToPDF(dataToExport);
            }
        },
        exportToCSV(filteredData) {
            const headers = ["ID", "Username", "Name", "Email", "Phone Number", "Gender", "Date of Birth", "Shipping Addresses", "Created Time"];
            const rows = filteredData.map(customer => [
                customer.id,
                customer.username,
                customer.name,
                customer.email,
                customer.phoneNumber,
                customer.gender,
                this.formatDate(customer.dob),
                customer.shippingAddresses.join('; '),
                this.formatCreatedTime(customer.createdTime)
            ]);

            let csvContent = "data:text/csv;charset=utf-8,"
                + headers.join(",") + "\n"
                + rows.map(e => e.join(",")).join("\n");

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "customers_report.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        },
        exportToPDF(filteredData) {
            const doc = new jsPDF();
            doc.text("Customers Report", 20, 10);
            doc.autoTable({
                head: [["ID", "Username", "Name", "Email", "Phone Number", "Gender", "Date of Birth", "Shipping Addresses", "Created Time"]],
                body: filteredData.map(customer => [
                    customer.id,
                    customer.username,
                    customer.name,
                    customer.email,
                    customer.phoneNumber,
                    customer.gender,
                    this.formatDate(customer.dob),
                    customer.shippingAddresses.join('; '),
                    this.formatCreatedTime(customer.createdTime)
                ])
            });
            doc.save("customers_report.pdf");
        },
        resetFilters() {
            this.searchQuery = "";
            this.filterGender = "";
            this.filterDOB = "";
            this.filterDate = "";
        },
        editCustomer(customer) {
            this.editCustomerId = customer.id; // Set the customer ID being edited
            this.editCustomerData = { ...customer, shippingAddresses: customer.shippingAddresses.join('; ') }; // Prepare data for editing
        },
        saveCustomer(customerId) {
            const index = this.customers.findIndex(customer => customer.id === customerId);
            if (index !== -1) {
                this.customers[index].name = this.editCustomerData.name;
                this.customers[index].email = this.editCustomerData.email;
                this.customers[index].shippingAddresses = this.editCustomerData.shippingAddresses.split(';').map(addr => addr.trim());
                this.customers[index].createdTime = new Date().toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
            }
            this.editCustomerId = null; // Reset the edit state
            this.editCustomerData = {};
        },
        deleteCustomer(customerId) {
            this.customers = this.customers.filter(customer => customer.id !== customerId);
        },
        cancelEdit() {
            this.editCustomerId = null; // Reset the edit state
            this.editCustomerData = {};
        },
        formatCreatedTime(dateTime) {
            return new Date(dateTime).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
        },
        formatDate(date) {
            return new Date(date).toLocaleDateString('en-US', { dateStyle: 'short' });
        }
    },
};
</script>


<style scoped>
.category {
    height: 7vh;
}

.table {
    margin-top: 10px;
}

.text-center {
    text-align: center;
}

.card-body {
    padding: 1rem;
}

.card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.btn-success {
    margin-right: 10px;
}
</style>
