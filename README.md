# iamdumb
For when you are trying to figure out why your AWS api called failed.

Prints this diagram in ascii art on your terminal:
![image](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/PolicyEvaluationHorizontal111621.png)
```
python3 ./iamdumb.py 
```

Add an alias for 'iamdumb' to your terminal.
```
echo "alias iamdumb='python3 $(pwd)/iamdumb.py'" >> ~/.zshrc && source ~/.zshrc
```

Alternatively, you may install it via pipx with:
```
pipx install git+https://github.com/lmoratti/iamdumb.git@main
```

Usage:
```
sh-5.2$ iamdumb

+-------------------+
|  Decision starts   |
|    with Deny       |
+---------+---------+
          |
+---------v---------+
| Evaluate all      |
| applicable        |
| policies          |
+---------+---------+
          |
+---------v---------+
| Is there an       |
| explicit Deny?    |
+---------+---------+
     | Yes         | No
     v             |
+---------+        v
| Final   |  +-----v-------+
| Decision:|  | Is the principal's  |
| Deny     |  | account a member of |
| (Explicit|  | an organization     |
| Deny)    |  | with an applicable  |
+---------+   | SCP?                |
              +-----+-------+
                    | Yes   | No
                    v       v
              +-----v---+ +---v------------+
              | Is there | | Final Decision:|
              | an Allow?| | Deny (Implicit)|
              +-----+---+ +----------------+
                    | No
                    v
            +-------v-------+
            | Final Decision:|
            | Deny (Implicit)|
            +---------------+

+-------------------+
| Does the requested |
| resource have a    |
| resource-based     |
| policy?            |
+---------+---------+
     | Yes         | No
     v             |
+---------+        v
| See     |  +-----v-------+
| Resource|  | Does the      |
| -based  |  | principal     |
| Policies|  | have an       |
| Section |  | identity-based|
+---------+  | policy?       |
              +-----+-------+
                    | Yes   | No
                    v       v
              +-----v---+ +---v------------+
              | Is there | | Final Decision:|
              | an Allow?| | Deny (Implicit)|
              +-----+---+ +----------------+
                    | No
                    v
            +-------v-------+
            | Final Decision:|
            | Deny (Implicit)|
            +---------------+

+-------------------+
| Does the principal |
| have permissions   |
| boundaries?        |
+---------+---------+
     | Yes         | No
     v             |
+---------+        v
| Is there |  +-----v-------+
| an Allow?|  | Final        |
+---------+  | Decision:     |
     | No     | Deny (Implicit)|
     v        +---------------+
+---------+
| Final   |
| Decision:|
| Deny    |
| (Implicit)|
+---------+

+-------------------+
| Is the principal   |
| a session principal|
+---------+---------+
     | Yes         | No
     v             |
+---------+        v
| Is this |  +-----v-------+
| a role  |  | Final        |
| session?|  | Decision:    |
+---------+  | Deny (Implicit)|
     | No     +---------------+
     v
+---------+
| Is there |
| a session|
| policy?  |
+---------+
     | Yes         | No
     v             v
+---------+    +---v------------+
| Is there |    | Final Decision:|
| an Allow?|    | Deny (Implicit)|
+---------+    +----------------+
     | No
     v
+---------+
| Final   |
| Decision:|
| Deny    |
| (Implicit)|
+---------+

+-------------------+
| Final Decision:    |
| Allow              |
+-------------------+
```
