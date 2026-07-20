# Discount Coupon Engine

## Introduction

The Discount Coupon Engine is a software system designed to manage and apply various promotional offers and discounts in an e-commerce environment. Modern online shopping platforms frequently offer discounts such as seasonal sales, loyalty rewards, bank-specific offers, and bulk purchase incentives. As the number and complexity of these promotional schemes increase, implementing them using simple conditional statements becomes difficult to maintain and extend.

The primary objective of this system is to provide a flexible, scalable, maintainable, and extensible architecture for managing discounts and coupons. Instead of tightly coupling discount logic with shopping cart functionality, the system separates responsibilities into different classes and modules. This separation follows fundamental Object-Oriented Programming (OOP) principles and employs well-known design patterns such as Strategy Pattern, Singleton Pattern, and Chain of Responsibility Pattern.

The architecture is designed to mimic real-world e-commerce platforms where multiple discounts may coexist and new promotional schemes need to be introduced frequently without affecting existing functionality.

## System Overview

At a high level, the system consists of four major components:

- Product and Cart Management
- Coupon Management
- Discount Calculation Mechanism
- Coupon Processing Framework

The interaction among these components enables the system to determine which discounts are applicable, calculate discount values, and produce the final payable amount for a customer.

The overall workflow begins when a customer adds products to a shopping cart. The system calculates the cart's initial value and then evaluates all available coupons. Each coupon verifies whether it can be applied to the current cart. Eligible coupons calculate their discount amount using predefined discount strategies. Finally, the calculated discounts are applied, and the final amount is generated.

## Product and Cart Management

The foundation of the system lies in representing products and customer purchases.

A Product represents an item available for purchase. Every product contains basic information such as its name, category, and price. The category attribute is particularly important because many promotional offers are category-specific. For example, an electronics discount should only apply to products classified under the electronics category.

When a customer adds a product to the cart, the product is encapsulated within a CartItem object. A CartItem combines a product with a quantity. This design separates product information from purchase information, making it possible to represent multiple quantities of the same product efficiently.

The Cart acts as the central aggregation point of the system. It stores all cart items and maintains information such as the initial total amount and final total amount after discounts are applied. The cart is responsible for calculating the total value of products before any promotional rules are considered.

From a software engineering perspective, the cart serves as the primary domain object on which all coupon validation and discount calculations operate.

## Coupon Framework

The core functionality of the system revolves around coupons and promotional offers.

Rather than creating separate and unrelated discount mechanisms, the system introduces an abstract Coupon class. This abstraction establishes a common contract that all coupon types must follow.

Every coupon must answer four fundamental questions:

1. Is the coupon applicable to the current cart?
2. How much discount should be provided?
3. Can the coupon be combined with other coupons?
4. How should the discount be applied?

This abstraction allows the system to treat all coupon types uniformly while enabling each coupon to implement its own business rules.

The Coupon class therefore acts as a blueprint from which all specialized coupons are derived. This design promotes consistency and reduces code duplication across different promotional schemes.

## Specialized Coupon Types

The system extends the Coupon abstraction into several concrete implementations that represent common business promotions.

### Seasonal Offer

A seasonal offer provides discounts during specific events or festivals. Examples include Diwali sales, Christmas offers, New Year promotions, and Black Friday discounts.

The applicability of a seasonal offe`r may depend on product categories or predefined promotional periods. Such offers are extremely common in retail environments and often target specific product groups.

### Loyalty Discount

Customer retention is a major objective of modern businesses. Loyalty discounts reward customers who repeatedly purchase products or subscribe to membership programs.

The loyalty discount coupon checks whether the customer qualifies for loyalty benefits. If the eligibility criteria are satisfied, the discount is applied.

This approach helps businesses encourage long-term customer engagement and increase customer lifetime value.

### Bulk Purchase Discount

Bulk purchase discounts are intended to encourage customers to buy larger quantities or spend beyond a specified threshold.

For example:

Spend ₹5000 and receive ₹500 off.
Buy 10 items and receive a special discount.

The coupon verifies whether the purchase exceeds the predefined threshold before applying the discount.

This strategy is widely used to increase average order value and maximize revenue.

### Bank Coupon

Bank coupons are commonly offered through partnerships between retailers and financial institutions.

Examples include:

₹500 cashback on HDFC cards.
10% instant discount on ICICI Bank credit cards.

The coupon validates factors such as:

Bank name
Minimum spending amount
Payment eligibility

Only after satisfying these conditions is the discount applied.

## Discount Strategy Architecture

One of the most significant aspects of the system is the implementation of the Strategy Pattern.

In traditional systems, discount formulas are often hardcoded directly within coupon classes. This approach creates several problems:

Difficult maintenance
Code duplication
Poor scalability
Violation of Open/Closed Principle

To overcome these limitations, the system separates discount calculation logic from coupon logic.

The abstraction responsible for this separation is the DiscountStrategy interface.

The interface defines a common method responsible for calculating discount values.

Rather than embedding mathematical calculations within coupon classes, each coupon delegates discount computation to a strategy object.

This design allows the same coupon type to use different discount calculation mechanisms without modifying coupon code.

## Types of Discount Strategies

The system supports multiple discount calculation techniques.

### Flat Discount Strategy

The flat discount strategy provides a fixed monetary reduction regardless of the cart amount.

Examples:

₹100 off
₹500 off

The discount value remains constant and independent of the cart total.

This strategy is simple and predictable.

### Percentage Discount Strategy

The percentage discount strategy calculates discounts as a percentage of the cart amount.

Examples:

10% off
20% off

The discount grows proportionally with the purchase amount.

This strategy is widely used because it scales naturally with order value.

### Percentage with Cap Strategy

A percentage-based discount may sometimes become excessively large for expensive purchases.

To prevent excessive losses, businesses often introduce a maximum discount limit.

Examples:

20% off up to ₹500
15% off up to ₹1000

The strategy calculates the percentage discount and then compares it with the maximum allowed discount. The smaller value is selected.

This approach balances customer benefits with business profitability.

## Strategy Manager

The creation and management of discount strategies are centralized within the DiscountStrategyManager.

This component serves as a factory-like mechanism responsible for providing the appropriate discount strategy based on user requirements.

Instead of creating strategy objects manually throughout the application, the system requests the required strategy from the manager.

This design provides several benefits:

Centralized object creation
Reduced coupling
Easier maintenance
Better scalability

If a new discount strategy is introduced in the future, modifications are required only within the strategy manager rather than across the entire application.

## Coupon Manager

The CouponManager serves as the central authority responsible for managing coupons.

Its responsibilities include:

Registering coupons
Storing coupon information
Identifying applicable coupons
Applying coupons to carts

The manager maintains a collection of coupons and evaluates them against the current cart.

By centralizing coupon processing, the system avoids duplication of validation logic and ensures consistency across the application.

The CouponManager follows the Singleton Pattern, ensuring that only one instance exists throughout the application's lifecycle.

This is important because coupon information represents shared application-wide data that should be managed centrally.

## Chain of Responsibility Mechanism

The Coupon class contains a reference to another coupon object.

This design effectively creates a linked chain of coupons.

Each coupon in the chain performs three actions:

Evaluate eligibility.
Apply discount if eligible.
Forward processing to the next coupon.

This behavior resembles the Chain of Responsibility Design Pattern.

The pattern provides several advantages:

Flexible coupon ordering
Simplified coupon stacking
Easy addition of new coupon types
Reduced coupling between coupon implementations

Instead of a monolithic discount engine, the system processes coupons incrementally through the chain.

## Object-Oriented Design Principles

The architecture strongly adheres to fundamental OOP principles.

### Encapsulation

Each class manages its own data and behavior. Internal implementation details are hidden from other components.

### Abstraction

The Coupon and DiscountStrategy abstractions define common contracts while hiding implementation complexity.

### Inheritance

Specialized coupon classes inherit common functionality from the Coupon base class.

### Polymorphism

Different coupon types and discount strategies can be treated uniformly through their abstract interfaces.

These principles collectively improve maintainability, readability, and extensibility.

## Design Patterns Used

The project demonstrates practical applications of several software design patterns.

### Strategy Pattern

Used to encapsulate different discount calculation algorithms.

Benefits:

Runtime flexibility
Easy extensibility
Reduced code duplication

### Singleton Pattern

Used in:

CouponManager
DiscountStrategyManager

Benefits:

Single source of truth
Controlled access
Reduced memory usage

### Chain of Responsibility Pattern

Used in coupon processing.

Benefits:

Dynamic coupon chains
Decoupled request handling
Easier feature expansion

## Scalability and Extensibility

One of the strongest characteristics of this architecture is its scalability.

Suppose a business introduces a new promotional scheme such as:

Student Discount
Referral Bonus
First Purchase Discount
Festival Cashback
Corporate Membership Discount

A developer only needs to create a new coupon class extending the Coupon abstraction.

The existing classes remain unchanged.

Similarly, if a new discount calculation method is introduced, developers simply create another implementation of DiscountStrategy.

This adherence to the Open/Closed Principle ensures long-term maintainability.
