# Smart Petrol Pump Management System

Below is a detailed description of each schema in the provided textual schema diagram for the petrol pump management system with a subscription system. This description explains the purpose of each schema, its fields, and its relationships with other schemas.

---

### 1. PetrolPump

**Description**: The main schema representing a petrol pump entity. It serves as the central hub connecting all other aspects of the system, including subscription details, pumps, fuel types, transactions, employees, inventory, billing history, and maintenance logs.

-   **Fields**:

    -   `name: String` - The name of the petrol pump (e.g., "City Fuel Station").
    -   `location: { type: "Point", coordinates: [Number] }` - Geospatial data with longitude and latitude for location-based queries.
    -   `owner: { name: String, contact: String, email: String }` - Details of the petrol pump owner.
    -   `status: String` - Current status of the petrol pump (e.g., "Active", "Inactive", "Pending").
    -   `createdAt: Date` - Timestamp of creation.
    -   `updatedAt: Date` - Timestamp of last update.

-   **Relationships**:
    -   `subscription: Subscription` - Links to the Subscription schema for subscription details.
    -   `pumps: [Pump]` - Array of references to Pump schemas (one-to-many).
    -   `fuelTypes: [FuelType]` - Array of references to FuelType schemas (one-to-many).
    -   `transactions: [Transaction]` - Array of references to Transaction schemas (one-to-many).
    -   `employees: [Employee]` - Array of references to Employee schemas (one-to-many).
    -   `inventory: [Inventory]` - Array of references to Inventory schemas (one-to-many).
    -   `billingHistory: [BillingHistory]` - Array of references to BillingHistory schemas (one-to-many).
    -   `maintenanceLogs: [MaintenanceLog]` - Array of references to MaintenanceLog schemas (one-to-many).

---

### 2. SubscriptionPlan

**Description**: Defines the available subscription plans for petrol pump management, specifying pricing, duration, and features.

-   **Fields**:

    -   `planName: String` - Name of the plan (e.g., "Basic", "Standard", "Premium").
    -   `price: Number` - Cost of the subscription plan.
    -   `duration: Number` - Duration of the plan in days.
    -   `features: [String]` - List of features included in the plan (e.g., "Inventory Tracking", "Employee Management").
    -   `maxUsers: Number` - Maximum number of users allowed.
    -   `maxPumps: Number` - Maximum number of pumps allowed under this plan.

-   **Relationships**: None (standalone schema).

---

### 3. Subscription

**Description**: Tracks an individual petrol pump's subscription instance, linking it to a specific plan and managing its duration and payment status.

-   **Fields**:

    -   `startDate: Date` - Start date of the subscription.
    -   `endDate: Date` - End date of the subscription.
    -   `paymentStatus: String` - Status of payment (e.g., "Pending", "Paid", "Failed").
    -   `transactionId: String` - Identifier for the payment transaction.

-   **Relationships**:
    -   `plan: SubscriptionPlan` - References the SubscriptionPlan schema.

---

### 4. FuelType

**Description**: Manages the different types of fuel available at the petrol pump, including pricing and quantity.

-   **Fields**:

    -   `name: String` - Type of fuel (e.g., "Petrol", "Diesel", "CNG", "Electric").
    -   `pricePerLiter: Number` - Current price per liter.
    -   `availableQuantity: Number` - Current stock available.

-   **Relationships**: None (referenced by other schemas).

---

### 5. Pump

**Description**: Represents individual fuel pumps at the petrol pump, linking each to a specific fuel type.

-   **Fields**:

    -   `pumpId: String` - Unique identifier for the pump.
    -   `status: String` - Current status (e.g., "Active", "Maintenance", "Inactive").

-   **Relationships**:
    -   `fuelType: FuelType` - References the FuelType schema.

---

### 6. Transaction

**Description**: Records fuel sales transactions, including quantity, amount, and payment details.

-   **Fields**:

    -   `quantity: Number` - Amount of fuel dispensed (in liters).
    -   `totalAmount: Number` - Total cost of the transaction.
    -   `paymentMethod: String` - Method of payment (e.g., "Cash", "Card", "UPI").
    -   `timestamp: Date` - Date and time of the transaction.

-   **Relationships**:
    -   `pumpId: Pump` - References the Pump schema.
    -   `fuelType: FuelType` - References the FuelType schema.

---

### 7. Employee

**Description**: Manages employee details, including their role, contact information, and work schedule.

-   **Fields**:

    -   `employeeId: String` - Unique identifier for the employee.
    -   `name: String` - Employee's name.
    -   `role: String` - Job role (e.g., "Manager", "Attendant", "Cashier", "Maintenance").
    -   `contact: String` - Phone number.
    -   `email: String` - Email address.
    -   `shift: { startTime: Date, endTime: Date }` - Work shift schedule.
    -   `status: String` - Current status (e.g., "Active", "Inactive", "OnLeave").
    -   `hireDate: Date` - Date of hiring.

-   **Relationships**: None (referenced by other schemas).

---

### 8. Inventory

**Description**: Tracks fuel inventory received from suppliers, including quantity and cost.

-   **Fields**:

    -   `quantityReceived: Number` - Amount of fuel received.
    -   `supplier: { name: String, contact: String }` - Supplier details.
    -   `receivedDate: Date` - Date of receipt.
    -   `unitCost: Number` - Cost per unit of fuel.

-   **Relationships**:
    -   `fuelType: FuelType` - References the FuelType schema.

---

### 9. BillingHistory

**Description**: Maintains a record of billing and payment history related to subscriptions.

-   **Fields**:

    -   `amount: Number` - Billing amount.
    -   `paymentDate: Date` - Date of payment.
    -   `paymentMethod: String` - Payment method (e.g., "Card", "BankTransfer", "Cash").
    -   `status: String` - Payment status (e.g., "Paid", "Pending", "Failed").
    -   `invoiceNumber: String` - Unique invoice identifier.

-   **Relationships**:
    -   `subscription: Subscription` - References the Subscription schema.

---

### 10. MaintenanceLog

**Description**: Logs maintenance activities for pumps, including costs and technician assignment.

-   **Fields**:

    -   `description: String` - Details of the maintenance work.
    -   `maintenanceDate: Date` - Date of maintenance.
    -   `cost: Number` - Cost of maintenance.
    -   `status: String` - Maintenance status (e.g., "Scheduled", "InProgress", "Completed").
    -   `duration: Number` - Time taken (in hours).

-   **Relationships**:
    -   `pumpId: Pump` - References the Pump schema.
    -   `technician: Employee` - References the Employee schema.

---

### Relationships Overview

The `PetrolPump` schema acts as the central entity, linking to:

-   A single `Subscription` (which references a `SubscriptionPlan`).
-   Multiple `Pump`s (each referencing a `FuelType`).
-   Multiple `FuelType`s.
-   Multiple `Transaction`s (each referencing a `Pump` and `FuelType`).
-   Multiple `Employee`s.
-   Multiple `Inventory` entries (each referencing a `FuelType`).
-   Multiple `BillingHistory` entries (each referencing a `Subscription`).
-   Multiple `MaintenanceLog`s (each referencing a `Pump` and an `Employee`).

This structure ensures a comprehensive system for managing a petrol pump, including subscriptions, operations, staff, inventory, billing, and maintenance. Let me know if you need further clarification or adjustments!

```
// schemas_diagram.js
// Note: This is a textual representation of schema relationships
// Format: SchemaName { fields, [relationships] }

const Schemas = {
  // Main Schema
  PetrolPump: {
    fields: {
      name: "String",
      location: {
        type: "Point",
        coordinates: "[Number]"
      },
      owner: {
        name: "String",
        contact: "String",
        email: "String"
      },
      status: "String",
      createdAt: "Date",
      updatedAt: "Date"
    },
    relationships: [
      "subscription: Subscription",
      "pumps: [Pump]",
      "fuelTypes: [FuelType]",
      "transactions: [Transaction]",
      "employees: [Employee]",
      "inventory: [Inventory]",
      "billingHistory: [BillingHistory]",
      "maintenanceLogs: [MaintenanceLog]"
    ]
  },

  // Subscription System
  SubscriptionPlan: {
    fields: {
      planName: "String",
      price: "Number",
      duration: "Number",
      features: "[String]",
      maxUsers: "Number",
      maxPumps: "Number"
    }
  },

  Subscription: {
    fields: {
      startDate: "Date",
      endDate: "Date",
      paymentStatus: "String",
      transactionId: "String"
    },
    relationships: [
      "plan: SubscriptionPlan"
    ]
  },

  // Fuel Management
  FuelType: {
    fields: {
      name: "String",
      pricePerLiter: "Number",
      availableQuantity: "Number"
    }
  },

  Pump: {
    fields: {
      pumpId: "String",
      status: "String"
    },
    relationships: [
      "fuelType: FuelType"
    ]
  },

  // Transactions
  Transaction: {
    fields: {
      quantity: "Number",
      totalAmount: "Number",
      paymentMethod: "String",
      timestamp: "Date"
    },
    relationships: [
      "pumpId: Pump",
      "fuelType: FuelType"
    ]
  },

  // Employee Management
  Employee: {
    fields: {
      employeeId: "String",
      name: "String",
      role: "String",
      contact: "String",
      email: "String",
      shift: {
        startTime: "Date",
        endTime: "Date"
      },
      status: "String",
      hireDate: "Date"
    }
  },

  // Inventory Tracking
  Inventory: {
    fields: {
      quantityReceived: "Number",
      supplier: {
        name: "String",
        contact: "String"
      },
      receivedDate: "Date",
      unitCost: "Number"
    },
    relationships: [
      "fuelType: FuelType"
    ]
  },

  // Billing History
  BillingHistory: {
    fields: {
      amount: "Number",
      paymentDate: "Date",
      paymentMethod: "String",
      status: "String",
      invoiceNumber: "String"
    },
    relationships: [
      "subscription: Subscription"
    ]
  },

  // Maintenance Logs
  MaintenanceLog: {
    fields: {
      description: "String",
      maintenanceDate: "Date",
      cost: "Number",
      status: "String",
      duration: "Number"
    },
    relationships: [
      "pumpId: Pump",
      "technician: Employee"
    ]
  }
};

/*
Relationships Diagram:
PetrolPump
├── Subscription → SubscriptionPlan
├── [Pump] → FuelType
├── [FuelType]
├── [Transaction] → Pump, FuelType
├── [Employee]
├── [Inventory] → FuelType
├── [BillingHistory] → Subscription
└── [MaintenanceLog] → Pump, Employee
*/
```

# Mongoose Schema:

Below is the merged version of the schemas, combining the original schemas with the additional features (validation middleware, enhanced employee management, inventory tracking, billing history, and maintenance logs) into a single, complete Mongoose schema file.

```javascript
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

// Subscription Plan Schema
const SubscriptionPlanSchema = new Schema({
    planName: {
        type: String,
        required: true,
        enum: ["Basic", "Standard", "Premium"],
    },
    price: {
        type: Number,
        required: true,
    },
    duration: {
        type: Number, // in days
        required: true,
    },
    features: [
        {
            type: String,
        },
    ],
    maxUsers: {
        type: Number,
        default: 1,
    },
    maxPumps: {
        type: Number,
        default: 1,
    },
});

// Subscription Schema
const SubscriptionSchema = new Schema({
    plan: {
        type: Schema.Types.ObjectId,
        ref: "SubscriptionPlan",
        required: true,
    },
    startDate: {
        type: Date,
        default: Date.now,
    },
    endDate: {
        type: Date,
        required: true,
    },
    paymentStatus: {
        type: String,
        enum: ["Pending", "Paid", "Failed"],
        default: "Pending",
    },
    transactionId: {
        type: String,
    },
});

// Fuel Type Schema
const FuelTypeSchema = new Schema({
    name: {
        type: String,
        required: true,
        enum: ["Petrol", "Diesel", "CNG", "Electric"],
    },
    pricePerLiter: {
        type: Number,
        required: true,
    },
    availableQuantity: {
        type: Number,
        default: 0,
    },
});

// Pump Schema
const PumpSchema = new Schema({
    pumpId: {
        type: String,
        required: true,
        unique: true,
    },
    fuelType: {
        type: Schema.Types.ObjectId,
        ref: "FuelType",
        required: true,
    },
    status: {
        type: String,
        enum: ["Active", "Maintenance", "Inactive"],
        default: "Active",
    },
});

// Transaction Schema
const TransactionSchema = new Schema({
    pumpId: {
        type: Schema.Types.ObjectId,
        ref: "Pump",
        required: true,
    },
    fuelType: {
        type: Schema.Types.ObjectId,
        ref: "FuelType",
        required: true,
    },
    quantity: {
        type: Number,
        required: true,
    },
    totalAmount: {
        type: Number,
        required: true,
    },
    paymentMethod: {
        type: String,
        enum: ["Cash", "Card", "UPI", "Other"],
        required: true,
    },
    timestamp: {
        type: Date,
        default: Date.now,
    },
});

// Enhanced Employee Schema
const EmployeeSchema = new Schema({
    employeeId: {
        type: String,
        required: true,
        unique: true,
    },
    name: {
        type: String,
        required: true,
        trim: true,
    },
    role: {
        type: String,
        enum: ["Manager", "Attendant", "Cashier", "Maintenance"],
        required: true,
    },
    contact: {
        type: String,
        required: true,
        match: [/^\+?[1-9]\d{1,14}$/, "Please enter a valid phone number"],
    },
    email: {
        type: String,
        required: true,
        match: [/.+\@.+\..+/, "Please enter a valid email"],
    },
    shift: {
        startTime: Date,
        endTime: Date,
    },
    status: {
        type: String,
        enum: ["Active", "Inactive", "OnLeave"],
        default: "Active",
    },
    hireDate: {
        type: Date,
        default: Date.now,
    },
});

// Inventory Schema
const InventorySchema = new Schema({
    fuelType: {
        type: Schema.Types.ObjectId,
        ref: "FuelType",
        required: true,
    },
    quantityReceived: {
        type: Number,
        required: true,
        min: [0, "Quantity cannot be negative"],
    },
    supplier: {
        name: String,
        contact: String,
    },
    receivedDate: {
        type: Date,
        default: Date.now,
    },
    unitCost: {
        type: Number,
        required: true,
        min: [0, "Cost cannot be negative"],
    },
});

// Billing History Schema
const BillingHistorySchema = new Schema({
    subscription: {
        type: Schema.Types.ObjectId,
        ref: "Subscription",
        required: true,
    },
    amount: {
        type: Number,
        required: true,
        min: [0, "Amount cannot be negative"],
    },
    paymentDate: {
        type: Date,
        default: Date.now,
    },
    paymentMethod: {
        type: String,
        enum: ["Card", "BankTransfer", "Cash", "UPI"],
        required: true,
    },
    status: {
        type: String,
        enum: ["Paid", "Pending", "Failed"],
        default: "Pending",
    },
    invoiceNumber: {
        type: String,
        required: true,
        unique: true,
    },
});

// Maintenance Log Schema
const MaintenanceLogSchema = new Schema({
    pumpId: {
        type: Schema.Types.ObjectId,
        ref: "Pump",
        required: true,
    },
    description: {
        type: String,
        required: true,
    },
    maintenanceDate: {
        type: Date,
        default: Date.now,
    },
    technician: {
        type: Schema.Types.ObjectId,
        ref: "Employee",
    },
    cost: {
        type: Number,
        min: [0, "Cost cannot be negative"],
    },
    status: {
        type: String,
        enum: ["Scheduled", "InProgress", "Completed", "Cancelled"],
        default: "Scheduled",
    },
    duration: {
        type: Number, // in hours
        min: [0, "Duration cannot be negative"],
    },
});

// Main Petrol Pump Schema
const PetrolPumpSchema = new Schema({
    name: {
        type: String,
        required: true,
        trim: true,
        minlength: [2, "Name must be at least 2 characters"],
    },
    location: {
        type: {
            type: String,
            enum: ["Point"],
            default: "Point",
        },
        coordinates: {
            type: [Number],
            required: true,
            validate: {
                validator: function (v) {
                    return (
                        v.length === 2 &&
                        v[0] >= -180 &&
                        v[0] <= 180 && // longitude
                        v[1] >= -90 &&
                        v[1] <= 90
                    ); // latitude
                },
                message: "Invalid coordinates",
            },
        },
    },
    owner: {
        name: { type: String, required: true },
        contact: {
            type: String,
            required: true,
            match: [/^\+?[1-9]\d{1,14}$/, "Please enter a valid phone number"],
        },
        email: {
            type: String,
            required: true,
            match: [/.+\@.+\..+/, "Please enter a valid email"],
        },
    },
    subscription: SubscriptionSchema,
    pumps: [{ type: Schema.Types.ObjectId, ref: "Pump" }],
    fuelTypes: [{ type: Schema.Types.ObjectId, ref: "FuelType" }],
    transactions: [{ type: Schema.Types.ObjectId, ref: "Transaction" }],
    employees: [{ type: Schema.Types.ObjectId, ref: "Employee" }],
    inventory: [{ type: Schema.Types.ObjectId, ref: "Inventory" }],
    billingHistory: [{ type: Schema.Types.ObjectId, ref: "BillingHistory" }],
    maintenanceLogs: [{ type: Schema.Types.ObjectId, ref: "MaintenanceLog" }],
    status: {
        type: String,
        enum: ["Active", "Inactive", "Pending"],
        default: "Pending",
    },
    createdAt: { type: Date, default: Date.now },
    updatedAt: { type: Date, default: Date.now },
});

// Indexes
PetrolPumpSchema.index({ location: "2dsphere" });

// Validation Middleware
PetrolPumpSchema.pre("save", async function (next) {
    this.updatedAt = Date.now();
    if (this.subscription && new Date(this.subscription.endDate) < new Date()) {
        this.status = "Inactive";
    }
    const plan = await SubscriptionPlan.findById(this.subscription.plan);
    if (plan && this.pumps.length > plan.maxPumps) {
        return next(new Error("Number of pumps exceeds subscription limit"));
    }
    next();
});

SubscriptionSchema.pre("save", function (next) {
    if (this.startDate > this.endDate) {
        return next(new Error("End date must be after start date"));
    }
    next();
});

InventorySchema.pre("save", async function (next) {
    const fuel = await FuelType.findById(this.fuelType);
    if (!fuel) {
        return next(new Error("Invalid fuel type"));
    }
    next();
});

// Create Models
const PetrolPump = mongoose.model("PetrolPump", PetrolPumpSchema);
const SubscriptionPlan = mongoose.model(
    "SubscriptionPlan",
    SubscriptionPlanSchema
);
const FuelType = mongoose.model("FuelType", FuelTypeSchema);
const Pump = mongoose.model("Pump", PumpSchema);
const Transaction = mongoose.model("Transaction", TransactionSchema);
const Employee = mongoose.model("Employee", EmployeeSchema);
const Inventory = mongoose.model("Inventory", InventorySchema);
const BillingHistory = mongoose.model("BillingHistory", BillingHistorySchema);
const MaintenanceLog = mongoose.model("MaintenanceLog", MaintenanceLogSchema);

module.exports = {
    PetrolPump,
    SubscriptionPlan,
    FuelType,
    Pump,
    Transaction,
    Employee,
    Inventory,
    BillingHistory,
    MaintenanceLog,
};
```

### Key Points of the Merge:

1. **Original Schemas**: The `SubscriptionPlanSchema`, `SubscriptionSchema`, `FuelTypeSchema`, `PumpSchema`, and `TransactionSchema` from the initial version are included unchanged at the top.
2. **New Schemas**: The enhanced `EmployeeSchema`, `InventorySchema`, `BillingHistorySchema`, and `MaintenanceLogSchema` are added as per the second version.
3. **Updated PetrolPumpSchema**: Combines the original fields with the new references (`employees`, `inventory`, `billingHistory`, `maintenanceLogs`) and validation rules from the second version.
4. **Validation Middleware**: Includes all pre-save hooks:
    - Updates `updatedAt` and checks subscription status in `PetrolPumpSchema`.
    - Validates subscription dates in `SubscriptionSchema`.
    - Checks fuel type validity in `InventorySchema`.
5. **Indexes**: Maintains the geospatial index for location queries.
6. **Models**: Exports all schemas as Mongoose models.

This merged file provides a complete, functional Mongoose schema for a petrol pump management system with all requested features (subscription system, validation, employee management, inventory, billing, and maintenance) in a single file. Let me know if you need further adjustments!
