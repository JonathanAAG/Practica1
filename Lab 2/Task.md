# Task 1: Analyze the Code

**Codigo inicial**

``` 
class SystemManager {
    processOrder(order) {
        if (order.type == "standard") {
            verifyInventory(order);
            processStandardPayment(order);
        } else if (order.type == "express") {
            verifyInventory(order);
            processExpressPayment(order, "highPriority");
        }
        updateOrderStatus(order, "processed");
        notifyCustomer(order);
    }
 
    verifyInventory(order) {
        // Checks inventory levels
        if (inventory < order.quantity) {
            throw new Error("Out of stock");
        }
    }
 
    processStandardPayment(order) {
        // Handles standard payment processing
        if (paymentService.process(order.amount)) {
            return true;
        } else {
            throw new Error("Payment failed");
        }
    }
 
    processExpressPayment(order, priority) {
        // Handles express payment processing
        if (expressPaymentService.process(order.amount, priority)) {
            return true;
        } else {
            throw new Error("Express payment failed");
        }
    }
 
    updateOrderStatus(order, status) {
        // Updates the order status in the database
        database.updateOrderStatus(order.id, status);
    }
 
    notifyCustomer(order) {
        // Sends an email notification to the customer
        emailService.sendEmail(order.customerEmail, "Your order has been processed.");
    }
}
``` 

### Analisis
Se identifica que no cumple con los siguientes principios

**Single Responsability:**  la clase SystemManager no cumple con el principio de solid que es el de unica responsabilidad ya que dentro de ella se tienen varias acciones diferentes como checar el inventario, pagos, notificaciones y ordenes.

**Open/closed**:  no cumple este principio ya que en caso de que se agregue otro tipo de pedido se tendria que alterar el codigo existente y por ultimo se viola el 

**Liskov Substitution**: no cumple ya que no maneja subtipos

**Interface Segregation Principle**: no cumple ya que no implementa interfaces

**Dependency Inversion**: no cumple ya que tiene una dependencia directa con las implementaciones de los metodos service y repository