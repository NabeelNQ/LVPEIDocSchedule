<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LVPEI Doctor Scheduling System</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        .container {
            width: 90%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        .header {
            background-color: #00704A; /* LVPEI green */
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .logo {
            font-weight: bold;
            font-size: 24px;
        }
        .user-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .user-avatar {
            width: 40px;
            height: 40px;
            background-color: #fff;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #00704A;
            font-weight: bold;
        }
        .nav {
            background-color: #f0f0f0;
            padding: 10px 20px;
            display: flex;
            gap: 20px;
        }
        .nav a {
            color: #333;
            text-decoration: none;
            font-weight: 500;
        }
        .nav a.active {
            color: #00704A;
            border-bottom: 2px solid #00704A;
        }
        .card {
            background-color: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            padding: 20px;
            margin-bottom: 20px;
        }
        .card h2 {
            margin-top: 0;
            color: #00704A;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }
        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }
        .dashboard-card {
            background-color: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            padding: 20px;
        }
        .dashboard-card h3 {
            margin-top: 0;
            color: #00704A;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        th {
            background-color: #f5f5f5;
        }
        tr:hover {
            background-color: #f9f9f9;
        }
        .button {
            background-color: #00704A;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }
        .button-secondary {
            background-color: #6c757d;
        }
        .button-small {
            padding: 5px 10px;
            font-size: 12px;
        }
        .tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 20px;
        }
        .tab {
            padding: 10px 15px;
            cursor: pointer;
        }
        .tab.active {
            border-bottom: 2px solid #00704A;
            color: #00704A;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }
        input, select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .doctor-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }
        .doctor-card {
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 15px;
            cursor: pointer;
        }
        .doctor-card.available {
            border-left: 4px solid #28a745;
        }
        .doctor-card.busy {
            border-left: 4px solid #dc3545;
        }
        .doctor-card.selected {
            background-color: #e6f7ff;
            border: 1px solid #1890ff;
        }
        .badge {
            display: inline-block;
            padding: 3px 7px;
            border-radius: 3px;
            font-size: 12px;
            font-weight: bold;
        }
        .badge-success {
            background-color: #d4edda;
            color: #155724;
        }
        .badge-danger {
            background-color: #f8d7da;
            color: #721c24;
        }
        .badge-warning {
            background-color: #fff3cd;
            color: #856404;
        }
        .badge-info {
            background-color: #d1ecf1;
            color: #0c5460;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }
        .modal-content {
            background-color: #fff;
            margin: 15% auto;
            padding: 20px;
            border-radius: 5px;
            width: 50%;
            max-width: 500px;
        }
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background-color: #00704A;
            color: white;
            padding: 15px 20px;
            border-radius: 4px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            z-index: 1000;
            display: none;
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="logo">LVPEI Doctor Scheduling System</div>
        <div class="user-info">
            <div class="user-avatar">DR</div>
            <div>Dr. Rajesh Kumar <small>(Ophthalmology)</small></div>
        </div>
    </div>
    
    <div class="nav">
        <a href="#dashboard" class="active">Dashboard</a>
        <a href="#schedule">My Schedule</a>
        <a href="#leave">Leave Management</a>
        <a href="#department">Department View</a>
        <a href="#reports">Reports</a>
    </div>
    
    <div class="container">
        <h1>Dashboard</h1>
        
        <div class="dashboard">
            <div class="dashboard-card">
                <h3>Quick Summary</h3>
                <ul>
                    <li>Scheduled clinics this week: <strong>5</strong></li>
                    <li>On-call duties: <strong>2</strong> (Wed, Sat)</li>
                    <li>Pending leave approvals: <strong>0</strong></li>
                    <li>Coverage requests: <strong>1 new</strong></li>
                </ul>
                <button class="button">View Schedule</button>
            </div>
            
            <div class="dashboard-card">
                <h3>Leave Balance</h3>
                <table>
                    <tr>
                        <td>Casual Leave</td>
                        <td>8 days</td>
                    </tr>
                    <tr>
                        <td>Medical Leave</td>
                        <td>12 days</td>
                    </tr>
                    <tr>
                        <td>Conference Leave</td>
                        <td>5 days</td>
                    </tr>
                </table>
                <button class="button" onclick="openLeaveModal()">Apply for Leave</button>
            </div>
            
            <div class="dashboard-card">
                <h3>Coverage Requests</h3>
                <div style="max-height: 200px; overflow-y: auto;">
                    <table>
                        <tr>
                            <td><strong>Dr. Priya Sharma</strong></td>
                            <td>Mar 20-21, 2025</td>
                            <td>
                                <button class="button button-small" onclick="showNotification('You have accepted to cover for Dr. Priya Sharma')">Accept</button>
                                <button class="button button-small button-secondary">Decline</button>
                            </td>
                        </tr>
                        <tr>
                            <td><strong>Dr. Anand Verma</strong></td>
                            <td>Apr 5, 2025</td>
                            <td>
                                <button class="button button-small" onclick="showNotification('You have accepted to cover for Dr. Anand Verma')">Accept</button>
                                <button class="button button-small button-secondary">Decline</button>
                            </td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
        
        <div class="card">
            <h2>Upcoming Schedule</h2>
            <div class="tabs">
                <div class="tab active" onclick="changeTab(event, 'daily')">Daily View</div>
                <div class="tab" onclick="changeTab(event, 'weekly')">Weekly View</div>
                <div class="tab" onclick="changeTab(event, 'monthly')">Monthly View</div>
            </div>
            
            <div id="daily" class="tab-content active">
                <h3>Thursday, March 13, 2025</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Time</th>
                            <th>Clinic</th>
                            <th>Room</th>
                            <th>Status</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>09:00 - 11:00</td>
                            <td>General Ophthalmology</td>
                            <td>Clinic Room 3</td>
                            <td><span class="badge badge-success">Confirmed</span></td>
                            <td><button class="button button-small button-secondary">Find Cover</button></td>
                        </tr>
                        <tr>
                            <td>11:30 - 13:00</td>
                            <td>Retina Specialty</td>
                            <td>Clinic Room 5</td>
                            <td><span class="badge badge-success">Confirmed</span></td>
                            <td><button class="button button-small button-secondary">Find Cover</button></td>
                        </tr>
                        <tr>
                            <td>14:00 - 16:00</td>
                            <td>Surgery</td>
                            <td>OR 2</td>
                            <td><span class="badge badge-success">Confirmed</span></td>
                            <td><button class="button button-small button-secondary">Find Cover</button></td>
                        </tr>
                        <tr>
                            <td>16:30 - 18:00</td>
                            <td>Follow-up Consultations</td>
                            <td>Clinic Room 3</td>
                            <td><span class="badge badge-success">Confirmed</span></td>
                            <td><button class="button button-small button-secondary">Find Cover</button></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            
            <div id="weekly" class="tab-content">
                <h3>Week of March 10 - 16, 2025</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Day</th>
                            <th>Morning (09:00 - 13:00)</th>
                            <th>Afternoon (14:00 - 18:00)</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Monday</td>
                            <td>General Clinic</td>
                            <td>Research</td>
                            <td><span class="badge badge-success">Completed</span></td>
                        </tr>
                        <tr>
                            <td>Tuesday</td>
                            <td>Surgery</td>
                            <td>Surgery Follow-ups</td>
                            <td><span class="badge badge-success">Completed</span></td>
                        </tr>
                        <tr>
                            <td>Wednesday</td>
                            <td>Teaching</td>
                            <td>On-call</td>
                            <td><span class="badge badge-success">Completed</span></td>
                        </tr>
                        <tr>
                            <td>Thursday</td>
                            <td>General & Retina Clinic</td>
                            <td>Surgery & Follow-ups</td>
                            <td><span class="badge badge-info">In Progress</span></td>
                        </tr>
                        <tr>
                            <td>Friday</td>
                            <td>Administrative Meeting</td>
                            <td>Patient Consultations</td>
                            <td><span class="badge badge-warning">Upcoming</span></td>
                        </tr>
                        <tr>
                            <td>Saturday</td>
                            <td>On-call</td>
                            <td>On-call</td>
                            <td><span class="badge badge-warning">Upcoming</span></td>
                        </tr>
                        <tr>
                            <td>Sunday</td>
                            <td>Off</td>
                            <td>Off</td>
                            <td><span class="badge badge-warning">Upcoming</span></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            
            <div id="monthly" class="tab-content">
