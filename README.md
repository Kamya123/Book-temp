import React, { useState } from 'react';
import axios from 'axios';

const ManagePermRole = () => {
  const [permRoles, setPermRoles] = useState([]);
  const [selectedRoleNumber, setSelectedRoleNumber] = useState('');
  const [newPermRole, setNewPermRole] = useState({ roleNumber: '', roleName: '' });
  const [updatePermRole, setUpdatePermRole] = useState({ roleName: '' });
  const [isAddMode, setIsAddMode] = useState(true);
  const [message, setMessage] = useState('');
  const [error, setError] = useState('');

  const handleViewPermRoles = () => {
    axios.get('http://localhost:6060/api/permrole/1')
      .then(response => {
        setPermRoles(response.data);
        setMessage('Permission roles fetched successfully.');
        setError('');
      })
      .catch(error => {
        setError('Error fetching permission roles.');
        setMessage('');
      });
  };

  const handleAddPermRole = (e) => {
    e.preventDefault();

    axios.post('http://localhost:6060/api/permrole/post', newPermRole)
      .then(response => {
        setMessage(response.data.message);
        setError('');
        setPermRoles([...permRoles, newPermRole]);
        setNewPermRole({ roleNumber: '', roleName: '' });
      })
      .catch(error => {
        setError(error.response?.data?.message || 'Error adding permission role.');
        setMessage('');
      });
  };

  const handleUpdatePermRole = (e) => {
    e.preventDefault();

    axios.put(`http://localhost:6060/api/update/permrole/${selectedRoleNumber}`, updatePermRole)
      .then(response => {
        setMessage(response.data.message);
        setError('');

        // Update local state
        const updatedRoles = permRoles.map(role => {
          if (role.roleNumber === selectedRoleNumber) {
            return { ...role, roleName: updatePermRole.roleName };
          }
          return role;
        });

        setPermRoles(updatedRoles);
        setUpdatePermRole({ roleName: '' });
      })
      .catch(error => {
        setError(error.response?.data?.message || 'Error updating permission role.');
        setMessage('');
      });
  };

  const toggleView = () => {
    setIsAddMode(!isAddMode);
    setMessage('');
    setError('');
  };

  return (
    <div className="container mt-4">
      <h3 className="mb-4 text-center">Manage Permission Roles</h3>

      <div className="text-center mb-4">
        <button className="btn btn-primary" onClick={toggleView}>
          {isAddMode ? 'Update Permission Role' : 'Add Permission Role'}
        </button>
        <button className="btn btn-info ml-2" onClick={handleViewPermRoles}>
          View Permission Roles
        </button>
      </div>

      {isAddMode ? (
        <form onSubmit={handleAddPermRole} className="form-group">
          <h4 className="mb-3">Add Permission Role</h4>

          <div className="mb-3">
            <label htmlFor="roleNumber" className="form-label">Role Number:</label>
            <input
              type="text"
              id="roleNumber"
              className="form-control"
              value={newPermRole.roleNumber}
              onChange={(e) => setNewPermRole({ ...newPermRole, roleNumber: e.target.value })}
              required
            />
          </div>

          <div className="mb-3">
            <label htmlFor="roleName" className="form-label">Role Name:</label>
            <input
              type="text"
              id="roleName"
              className="form-control"
              value={newPermRole.roleName}
              onChange={(e) => setNewPermRole({ ...newPermRole, roleName: e.target.value })}
              required
            />
          </div>

          <button type="submit" className="btn btn-success">Add Role</button>
        </form>
      ) : (
        <form onSubmit={handleUpdatePermRole} className="form-group">
          <h4 className="mb-3">Update Permission Role</h4>

          <div className="mb-3">
            <label htmlFor="selectRole" className="form-label">Select Role Number:</label>
            <select
              id="selectRole"
              className="form-control"
              value={selectedRoleNumber}
              onChange={(e) => setSelectedRoleNumber(e.target.value)}
              required
            >
              <option value="">Select Role</option>
              {permRoles.map(role => (
                <option key={role.roleNumber} value={role.roleNumber}>
                  {role.roleNumber} - {role.roleName}
                </option>
              ))}
            </select>
          </div>

          <div className="mb-3">
            <label htmlFor="updateRoleName" className="form-label">New Role Name:</label>
            <input
              type="text"
              id="updateRoleName"
              className="form-control"
              value={updatePermRole.roleName}
              onChange={(e) => setUpdatePermRole({ roleName: e.target.value })}
              required
            />
          </div>

          <button type="submit" className="btn btn-warning">Update Role</button>
        </form>
      )}

      {permRoles.length > 0 && (
        <div className="mt-4">
          <h4>Existing Permission Roles</h4>
          <ul className="list-group">
            {permRoles.map(role => (
              <li key={role.roleNumber} className="list-group-item">
                {role.roleNumber} - {role.roleName}
              </li>
            ))}
          </ul>
        </div>
      )}

      {message && <div className="alert alert-success mt-3">{message}</div>}
      {error && <div className="alert alert-danger mt-3">{error}</div>}
    </div>
  );
};

export default ManagePermRole;

Sidebar:
{ to: "/admin-login/perm-role", label: "Permisions", icon: <FaChartLine /> },


//App.jsx:
import ManagePermRole from "./components/adminDashboard/component/permRole/ManagePermRole";
<Route path="/admin-login" element={<AdminDashboard />}>
          {/* <Route index element={<AdminSummary />} /> */}
          <Route index element={<ManageBooks />} />
          <Route path="users" element={<ManageUsers />} />
          <Route path="purchase-log" element={<ManagePurchaseLog />} />
          <Route path="inventory" element={<ManageInventory />} />
          <Route path="perm-role" element={<ManagePermRole />} />
        </Route>

