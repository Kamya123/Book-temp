```
import React, { useState, useEffect } from "react";
import { SiBookstack } from "react-icons/si";
import { useNavigate } from "react-router-dom";

const LandingPage = () => {
  const [showDropdown, setShowDropdown] = useState(false);
  const navigate = useNavigate();

  useEffect(() => {
    const closeDropdown = (e) => {
      if (
        !e.target.closest(".dropdown-menu") &&
        !e.target.closest(".dropdown-toggle")
      ) {
        setShowDropdown(false);
      }
    };

    document.addEventListener("click", closeDropdown);
    return () => document.removeEventListener("click", closeDropdown);
  }, []);

  const handleLogin = (role) => {
    navigate(`/login?role=${role}`);
  };

  return (
    <div className="d-flex flex-column min-vh-100">
      {/* Header Section */}
      <header className="bg-dark bg-opacity-75 text-white py-3 fixed-top shadow">
        <div className="container d-flex justify-content-between align-items-center">
          {/* Logo */}
          <div className="d-flex align-items-center">
            <SiBookstack className="text-primary fs-3 me-2" />
            <h1 className="h4 mb-0">BookTrack</h1>
          </div>

          {/* Navigation */}
          <nav className="d-flex gap-4">
            <a
              href="#home"
              className="text-white text-decoration-none px-3 fs-5 py-2"
            >
              Home
            </a>
            <a
              href="#features"
              className="text-white text-decoration-none px-3 fs-5 py-2"
            >
              Features
            </a>
            <a
              href="#contact"
              className="text-white text-decoration-none px-3 fs-5 py-2"
            >
              Contact
            </a>
          </nav>
        </div>
      </header>

      {/* Main Content */}
      <main className="flex-grow-1">
        {/* Welcome Section */}
        <section
          id="home"
          className="welcome-section text-center text-white d-flex align-items-center justify-content-center"
          style={{
            backgroundImage:
              "url('https://images.unsplash.com/photo-1524995997946-a1c2e315a42f?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&q=80&w=1080')",
            backgroundSize: "cover",
            backgroundPosition: "center",
            height: "100vh",
          }}
        >
          <div className="bg-dark bg-opacity-75 p-4 rounded mt-5">
            <h2 className="display-4 fw-bold mb-4">Welcome to BookTrack</h2>
            <p className="lead mb-4">
              Manage your library, bookstore, or collection effortlessly.
            </p>
            <div className="dropdown position-relative">
              <button
                className="btn btn-primary dropdown-toggle"
                type="button"
                onClick={() => setShowDropdown(!showDropdown)}
              >
                Login
              </button>
              <ul
                className={`dropdown-menu position-absolute${
                  showDropdown ? " show" : ""
                }`}
                style={{ top: "100%", left: "0" }}
              >
                <li>
                  <a
                    className="dropdown-item"
                    href="#"
                    onClick={() => handleLogin("Admin")}
                  >
                    Admin Login
                  </a>
                </li>
                <li>
                  <a
                    className="dropdown-item"
                    href="#"
                    onClick={() => handleLogin("User")}
                  >
                    User Login
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </section>
      </main>
    </div>
  );
};

export default LandingPage;
```
```
import React, { useState, useEffect } from "react";
import { useNavigate, useLocation } from "react-router-dom";
import queryString from "query-string";

const LoginPage = () => {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [role, setRole] = useState("");
  const navigate = useNavigate();
  const location = useLocation();

  useEffect(() => {
    const query = queryString.parse(location.search);
    if (query.role === "Admin") {
      setUsername("admin@me.com");
      setRole("Admin");
    } else if (query.role === "User") {
      setUsername("user@me.com");
      setRole("User");
    }
  }, [location.search]);

  const handleLogin = (e) => {
    e.preventDefault();
    if (role === "Admin" && username == "admin@me.com" && password == "123") {
      navigate("/admin-login");
    } else if (role === "User" && username == "user@me.com" && password == "123") {
      navigate("/user-login");
    } else {
      alert("Invalid login credentials");
    }
  };

  return (
    <div className="min-vh-100 d-flex align-items-center justify-content-center bg-light">
      <div className="card shadow-lg p-4" style={{ width: "24rem" }}>
        <h2 className="text-center text-primary mb-4">
          {role === "Admin" ? "Admin Login" : "User Login"}
        </h2>
        <form onSubmit={handleLogin}>
          {/* Username */}
          <div className="mb-3">
            <label htmlFor="username" className="form-label">
              Username
            </label>
            <input
              type="text"
              id="username"
              className="form-control"
              value={username}
              onChange={(e) => setUsername(e.target.value)}
              required
            />
          </div>

          {/* Password Input */}
          <div className="mb-3">
            <label htmlFor="password" className="form-label">
              Password
            </label>
            <input
              type="password"
              id="password"
              className="form-control"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
            />
          </div>

          {/* Login Button */}
          <button type="submit" className="btn btn-primary w-100">
            Login
          </button>
        </form>
      </div>
    </div>
  );
};

export default LoginPage;
```

