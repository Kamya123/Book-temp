UserSidebar.jsx:
complete code remove and paste this
```
import React from "react";
import {
  FaBook,
  FaCartPlus,
  FaSignOutAlt,
} from "react-icons/fa";
import { NavLink } from "react-router-dom";

const sidebarItems = [
  { to: "/user-login", label: "Books", icon: <FaBook /> },
  { to: "/user-login/shopping-cart", label: "Shopping Cart", icon: <FaCartPlus /> },
];

const UserSidebar = () => {
  return (
    <div
      className="h-100 position-fixed d-flex flex-column"
      style={{
        width: "16rem",
        height: "100vh",
        backgroundColor: "#f8f9fa", 
        borderRight: "1px solid #ddd", 
      }}
    >
      {/* Sidebar Header */}
      <div
        style={{
          padding: "1rem 0",
          textAlign: "center",
          borderBottom: "1px solid #ddd", 
          margin: "0 auto",
          width: "80%", 
        }}
      >
        <span style={{ color: "#6c757d", fontSize: "1.5rem", fontWeight: "500" }}>User Panel</span>
      </div>

      {/* Sidebar Navigation Links */}
      <div
        style={{
          flexGrow: 1,
          overflowY: "auto",
          padding: "1rem 0",
        }}
      >
        {sidebarItems.map((item, index) => (
          <NavLink
            key={index}
            to={item.to}
            end={item.to === "/user-login"}
            className={({ isActive }) =>
              `${
                isActive
                  ? "text-dark bg-light"
                  : "text-dark bg-transparent"
              } d-flex flex-column align-items-center justify-content-center mb-3 p-2 text-decoration-none rounded`
            }
            style={{
              fontSize: "0.9rem",
              transition: "background-color 0.2s ease-in-out, color 0.2s ease-in-out",
            }}
          >
            <div style={{ fontSize: "1.5rem", marginBottom: "0.5rem" }}>
              {item.icon}
            </div>
            <span>{item.label}</span>
          </NavLink>
        ))}
      </div>

      {/* Sidebar Footer */}
      <div
        style={{
          padding: "1rem 0",
          textAlign: "center",
          borderTop: "1px solid #ddd", 
        }}
      >
        <NavLink
          to="/logout"
          className="text-dark text-decoration-none d-flex flex-column align-items-center justify-content-center"
        >
          <FaSignOutAlt style={{ fontSize: "1.5rem", marginBottom: "0.5rem" }} />
          <span>Logout</span>
        </NavLink>
      </div>
    </div>
  );
};

export default UserSidebar;
```



