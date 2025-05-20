# Solution for SQL Server Connection Issues Over the Network

![](Solution/003.png)

## **Problem Statement**

When attempting to connect from a client (e.g., Windows 11) to a SQL Server instance **RUBINHOOD**, connection errors may occur. Error messages such as **"SQL Server does not exist or access denied"** or **"SQL Server Error: 17"** indicate that the server is unreachable.

The problem is often due to one of the following reasons:

- **TCP/IP protocol is not enabled**
- **SQL Server is listening on a dynamic port instead of 1433**
- **Firewall is blocking the SQL Server port**
- **SQL Server Browser service is disabled**

This article provides a step-by-step guide to resolving these issues based on a troubleshooting process with screenshots.

---

## **Step 1: Verify that TCP/IP is Enabled**

1. **Open SQL Server Configuration Manager** (Win + R, then enter `SQLServerManagerXX.msc`, where XX is the SQL version).
2. **Navigate to "SQL Server Network Configuration" > "Protocols for RUBINHOOD".**
3. Ensure that **TCP/IP is enabled**.
4. If TCP/IP is disabled, right-click > **Enable**.
5. Restart the SQL Server service.

*Result: SQL Server RUBINHOOD now accepts connections over TCP/IP.*

---

## **Step 2: Set a Static Port 1433**

![](Solution/002.jpg)

1. **Double-click on "TCP/IP" > Go to the "IP Addresses" tab.**
2. Scroll down to **"IPAll"**.
3. **Clear "TCP Dynamic Ports"** (must be empty).
4. **Set "TCP Port" to 1433.**
5. **Restart the SQL Server service.**

*Result: SQL Server RUBINHOOD now uses the standard port 1433, which is required for remote connections.*

---

## **Step 3: Configure Firewall for SQL Server RUBINHOOD**

1. **Open Windows PowerShell as Administrator** (Win + X > "PowerShell (Admin)").
2. Run the following command to allow the port through the firewall:
   ```powershell
   New-NetFirewallRule -DisplayName "Allow SQL Server 1433" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action Allow
   ```

*Result: SQL Server RUBINHOOD is now accessible over the network.*

---

## **Step 4: Test the Connection from a Client**

1. **Open PowerShell on another machine (e.g., Windows 11 client).**
2. Run the following command to test network connectivity:
   ```powershell
   Test-NetConnection -ComputerName 192.168.178.3 -Port 1433
   ```
3. If `TcpTestSucceeded = True`, the port is reachable.
4. If `False`, check the firewall and network settings again.

*Result: SQL Server RUBINHOOD is now accessible from the network.*

---

## **Step 5: Test Connection with SQL Server Management Studio (SSMS) or ODBC**

1. **Open SSMS or ODBC Data Source Administrator on the client.**
2. **Enter the server name:**
   - If default instance: `192.168.178.3`
   - If named instance RUBINHOOD: `192.168.178.3\RUBINHOOD`
   - If specifying the port explicitly: `192.168.178.3,1433`
3. Test the connection.

### **Example ODBC Error Message**
If the connection fails, you may see an error like this:

![](Solution/001.jpg)

This error typically indicates that:
- The server is unreachable due to a firewall or network issue.
- TCP/IP is not enabled.
- SQL Server is not listening on the correct port.

To resolve this, ensure that **TCP/IP is enabled**, the **firewall allows connections**, and the **correct port (1433) is set** in SQL Server Configuration Manager.

*Result: Successful connection to SQL Server RUBINHOOD over the network!*

---

## **Conclusion**

By correctly configuring **TCP/IP, setting a fixed port (1433), and adjusting firewall rules**, the connection issue was resolved. If errors persist, check:

- **Whether the SQL Server service is running** (`Get-Service -Name "MSSQL$RUBINHOOD"`)
- **If SQL Server authentication is properly configured**
- **If domain policies allow access to SQL Server RUBINHOOD**

Hopefully, this guide helps you resolve SQL Server RUBINHOOD network issues! 

