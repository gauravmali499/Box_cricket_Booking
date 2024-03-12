Here's a basic example demonstrating signup, login, and booking functionality using HTML for the front end and TypeScript for the back end. This example assumes you're using Node.js with Express for the server-side framework.

First, let's set up the directory structure:

lua
Copy code
box-cricket-booking/
|-- src/
    |-- controllers/
        |-- authController.ts
        |-- bookingController.ts
    |-- models/
        |-- User.ts
        |-- Booking.ts
    |-- routes/
        |-- authRoutes.ts
        |-- bookingRoutes.ts
    |-- views/
        |-- signup.html
        |-- login.html
        |-- booking.html
    |-- server.ts
|-- tsconfig.json
|-- package.json
Now, let's write some TypeScript code:

typescript
Copy code
// src/models/User.ts
export interface User {
  id: number;
  username: string;
  password: string;
  userType: UserType;
}

export enum UserType {
  Admin = 'admin',
  EndUser = 'end-user'
}
typescript
Copy code
// src/models/Booking.ts
export interface Booking {
  id: number;
  userId: number;
  date: Date;
  // Add more properties as needed
}
typescript
Copy code
// src/controllers/authController.ts
import { Request, Response } from 'express';
import { User, UserType } from '../models/User';

let users: User[] = [];

export const signup = (req: Request, res: Response) => {
  const { username, password, userType } = req.body;
  const newUser: User = {
    id: users.length + 1,
    username,
    password,
    userType
  };
  users.push(newUser);
  res.status(201).send('User signed up successfully');
};

export const login = (req: Request, res: Response) => {
  const { username, password } = req.body;
  const user = users.find((u) => u.username === username && u.password === password);
  if (user) {
    res.status(200).send('Login successful');
  } else {
    res.status(401).send('Invalid credentials');
  }
};
typescript
Copy code
// src/controllers/bookingController.ts
import { Request, Response } from 'express';
import { Booking } from '../models/Booking';

let bookings: Booking[] = [];

export const bookSlot = (req: Request, res: Response) => {
  // Handle booking logic here
};
typescript
Copy code
// src/routes/authRoutes.ts
import express from 'express';
import { signup, login } from '../controllers/authController';

const router = express.Router();

router.post('/signup', signup);
router.post('/login', login);

export default router;
typescript
Copy code
// src/routes/bookingRoutes.ts
import express from 'express';
import { bookSlot } from '../controllers/bookingController';

const router = express.Router();

router.post('/book', bookSlot);

export default router;
typescript
Copy code
// src/server.ts
import express from 'express';
import authRoutes from './routes/authRoutes';
import bookingRoutes from './routes/bookingRoutes';

const app = express();
const PORT = 3000;

app.use(express.json());
app.use('/auth', authRoutes);
app.use('/booking', bookingRoutes);

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
Now, let's create some simple HTML files for the views:

html
Copy code
<!-- src/views/signup.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Signup</title>
</head>
<body>
  <h1>Signup</h1>
  <form action="/auth/signup" method="POST">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required><br>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required><br>
    <label for="userType">User Type:</label>
    <select id="userType" name="userType">
      <option value="admin">Admin</option>
      <option value="end-user">End User</option>
    </select><br>
    <button type="submit">Signup</button>
  </form>
</body>
</html>
html
Copy code
<!-- src/views/login.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Login</title>
</head>
<body>
  <h1>Login</h1>
  <form action="/auth/login" method="POST">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required><br>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required><br>
    <button type="submit">Login</button>
  </form>
</body>
</html>
html
Copy code
<!-- src/views/booking.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Booking</title>
</head>
<body>
  <h1>Book Slot</h1>
  <form action="/booking/book" method="POST">
    <!-- Add booking form fields here -->
    <button type="submit">Book Slot</button>
  </form>
</body>
</html>
