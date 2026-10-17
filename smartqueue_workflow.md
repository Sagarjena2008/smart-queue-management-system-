                 START
                   |
                   v
          Open SmartQueue Website
                   |
                   v
           Customer Registration
                   |
                   v
        Enter Name, Phone, Service
                   |
                   v
          Validate Information
                   |
             +-----+-----+
             |           |
          Invalid       Valid
             |           |
             v           v
        Show Error   Generate Token
                         |
                         v
                   Save in MySQL
                         |
                         v
                  Display Token
                         |
                         v
                 Check Queue Status
                         |
                         v
                 Admin Calls Next
                         |
                         v
                  Status = SERVING
                         |
                         v
                Complete Customer
                         |
                         v
                 Status = COMPLETED
                         |
                         v
                        END
