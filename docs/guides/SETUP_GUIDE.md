# Setup Guide - Cloud-Based SDN Management Dashboard

This guide provides step-by-step instructions for setting up the complete development environment for the Cloud-Based SDN Management Dashboard project.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [OMNeT++ Installation](#omnetpp-installation)
3. [INET Framework Setup](#inet-framework-setup)
4. [SDN Framework Installation](#sdn-framework-installation)
5. [Dashboard Backend Setup](#dashboard-backend-setup)
6. [Dashboard Frontend Setup](#dashboard-frontend-setup)
7. [Verification and Testing](#verification-and-testing)
8. [Troubleshooting](#troubleshooting)
9. [Next Steps](#next-steps)

---

## Prerequisites

### Hardware Requirements
- **CPU:** 4+ cores (Intel i5/AMD Ryzen 5 or better)
- **RAM:** 8GB minimum, 16GB recommended
- **Storage:** 10GB free space
- **Network:** Internet connection for downloading dependencies

### Software Requirements

#### All Platforms
- **C++ Compiler:**
  - Linux: GCC 9+ or Clang 10+
  - macOS: Xcode Command Line Tools (includes Clang)
  - Windows: MinGW-w64 or MSVC 2019+
- **Make:** GNU Make 4.0+
- **Git:** Version control system
- **Python:** 3.8+ (for build scripts)

#### Dashboard Requirements
Choose **ONE** of the following:
- **Node.js:** 18.x or 20.x LTS (includes npm)
- **Python:** 3.9+ with pip (alternative backend option)

### Supported Operating Systems
- ✅ **Ubuntu 20.04/22.04 LTS** (Recommended)
- ✅ **macOS 11 Big Sur or later**
- ✅ **Windows 10/11** (with WSL2 recommended)
- ✅ **Fedora 36+**
- ✅ **Arch Linux** (latest)

---

## OMNeT++ Installation

OMNeT++ 7.0.0 is already present in this repository. Follow these steps to complete the setup.

### Step 1: Install Dependencies

#### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install -y build-essential gcc g++ bison flex perl \
    python3 python3-pip qt5-default libqt5opengl5-dev \
    libxml2-dev zlib1g-dev doxygen graphviz libwebkit2gtk-4.0-37
```

#### macOS
```bash
# Install Xcode Command Line Tools
xcode-select --install

# Install Homebrew if not present
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install dependencies
brew install bison flex python@3.11
brew install qt@5
```

#### Windows (WSL2)
```bash
# First, install WSL2 with Ubuntu 22.04 from Microsoft Store
# Then run Ubuntu commands above inside WSL2
```

### Step 2: Configure OMNeT++

```bash
# Navigate to OMNeT++ directory
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing/tj_omnet

# Configure (creates Makefile.inc based on detected libraries)
./configure

# Check configuration output for any warnings
# Should say "configure: OMNeT++ is configured successfully"
```

**Important Configuration Options:**
```bash
# If you need custom paths (usually not necessary):
./configure --with-qt=/path/to/qt --with-osgearth=no
```

### Step 3: Build OMNeT++

```bash
# Compile OMNeT++ (takes 10-30 minutes)
make -j4  # Use -j8 for 8 cores

# Expected output: "Build successful!"
```

### Step 4: Set Environment Variables

#### Linux/macOS (Add to `~/.bashrc` or `~/.zshrc`)
```bash
# OMNeT++ Environment
export OMNETPP_ROOT=/Users/justinbravo/Desktop/Coding/School/Cloud_Computing/tj_omnet
export PATH=$OMNETPP_ROOT/bin:$PATH
export LD_LIBRARY_PATH=$OMNETPP_ROOT/lib:$LD_LIBRARY_PATH  # Linux only

# Apply changes
source ~/.bashrc  # or source ~/.zshrc
```

#### Quick Test
```bash
# Or use the provided script
source $OMNETPP_ROOT/setenv

# Verify installation
omnetpp --version
# Should output: OMNeT++ Discrete Event Simulation (C) 1992-2025 Andras Varga...
```

### Step 5: Test OMNeT++ IDE (Optional)

```bash
# Launch OMNeT++ IDE
omnetpp

# If IDE doesn't open, ensure Qt5 is properly installed
```

---

## INET Framework Setup

INET provides network protocol implementations (TCP/IP, Ethernet, etc.) required for SDN simulation.

### Step 1: Download INET Framework

```bash
# Navigate to a convenient location (e.g., same directory as OMNeT++)
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing

# Clone INET Framework (version 4.5.x recommended for OMNeT++ 7.0)
git clone https://github.com/inet-framework/inet.git
cd inet

# Checkout stable version
git checkout v4.5.2  # or latest compatible version
```

### Step 2: Build INET

```bash
# Generate Makefiles
make makefiles

# Compile INET (takes 15-45 minutes)
make -j4

# Expected output: "INET build successful"
```

### Step 3: Verify INET Installation

```bash
# Run an example simulation
cd examples/inet/aloha
./aloha -u Cmdenv

# Should output: simulation statistics without errors
```

### Step 4: Configure INET Path

Add to your environment configuration:
```bash
export INET_ROOT=/Users/justinbravo/Desktop/Coding/School/Cloud_Computing/inet
```

---

## SDN Framework Installation

Choose **ONE** of the following SDN frameworks:

### Option A: OpenFlowOMNeTSuite (Recommended)

**Advantages:** Well-documented, supports OpenFlow 1.3, actively maintained

#### Step 1: Clone Repository
```bash
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing

git clone https://github.com/lsinfo3/OpenFlowOMNeTSuite.git
cd OpenFlowOMNeTSuite
```

#### Step 2: Check Compatibility
```bash
# Verify OMNeT++ version compatibility in README
cat README.md | grep -i "omnet"

# May need to checkout specific branch for OMNeT++ 7.0
git checkout master  # or specific branch
```

#### Step 3: Build OpenFlowOMNeTSuite
```bash
# Generate Makefiles (ensure INET_ROOT is set)
make makefiles

# Compile
make -j4

# Expected output: Build successful
```

#### Step 4: Run Example
```bash
cd simulations/networks
../run -u Cmdenv -c SimpleTopology

# Should execute OpenFlow scenario
```

#### Step 5: Set Environment Variable
```bash
export OPENFLOW_ROOT=/Users/justinbravo/Desktop/Coding/School/Cloud_Computing/OpenFlowOMNeTSuite
```

---

### Option B: SDN4CoRE (Alternative)

**Note:** SDN4CoRE focuses on real-time Ethernet SDN. May require porting for OMNeT++ 7.0.

#### Step 1: Clone Repository
```bash
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing

git clone https://github.com/CoRE-RG/SDN4CoRE.git
cd SDN4CoRE
```

#### Step 2: Check Compatibility and Build
```bash
# Follow instructions in SDN4CoRE README
# May need compatibility updates for OMNeT++ 7.0
```

**Recommendation:** Start with OpenFlowOMNeTSuite for easier compatibility.

---

## Dashboard Backend Setup

Choose **Node.js** (recommended) or **Python** for the backend API.

### Option A: Node.js Backend (Recommended)

#### Step 1: Install Node.js

##### Ubuntu/Debian
```bash
# Using NodeSource repository
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verify
node --version  # Should be v20.x.x
npm --version   # Should be 10.x.x
```

##### macOS
```bash
brew install node@20

# Verify
node --version
npm --version
```

##### Windows (WSL2)
```bash
# Same as Ubuntu instructions above
```

#### Step 2: Create Backend Project
```bash
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing/tj_omnet

# Create backend directory
mkdir -p dashboard/backend
cd dashboard/backend

# Initialize npm project
npm init -y

# Install dependencies
npm install express cors body-parser ws dotenv
npm install --save-dev nodemon

# Create directory structure
mkdir -p src/routes src/controllers src/models src/services
```

#### Step 3: Create Basic Server

Create `src/server.js`:
```javascript
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');
const WebSocket = require('ws');

const app = express();
const PORT = process.env.PORT || 8000;

// Middleware
app.use(cors());
app.use(bodyParser.json());

// Basic route
app.get('/api/health', (req, res) => {
    res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

// Start server
const server = app.listen(PORT, () => {
    console.log(`Backend server running on http://localhost:${PORT}`);
});

// WebSocket server for real-time updates
const wss = new WebSocket.Server({ server });
wss.on('connection', (ws) => {
    console.log('WebSocket client connected');
    ws.send(JSON.stringify({ type: 'welcome', message: 'Connected to SDN Dashboard' }));
});

module.exports = { app, wss };
```

#### Step 4: Update `package.json`
```json
{
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
  }
}
```

#### Step 5: Test Backend
```bash
npm run dev

# In another terminal:
curl http://localhost:8000/api/health
# Should return: {"status":"ok","timestamp":"..."}
```

---

### Option B: Python Backend (Alternative)

#### Step 1: Install Python and Dependencies

```bash
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing/tj_omnet

# Create backend directory
mkdir -p dashboard/backend
cd dashboard/backend

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # Linux/macOS
# venv\Scripts\activate  # Windows

# Install dependencies
pip install flask flask-cors flask-socketio python-socketio
```

#### Step 2: Create Basic Server

Create `server.py`:
```python
from flask import Flask, jsonify, request
from flask_cors import CORS
from flask_socketio import SocketIO, emit
import datetime

app = Flask(__name__)
CORS(app)
socketio = SocketIO(app, cors_allowed_origins="*")

@app.route('/api/health', methods=['GET'])
def health():
    return jsonify({
        'status': 'ok',
        'timestamp': datetime.datetime.now().isoformat()
    })

@socketio.on('connect')
def handle_connect():
    print('WebSocket client connected')
    emit('welcome', {'message': 'Connected to SDN Dashboard'})

if __name__ == '__main__':
    socketio.run(app, host='localhost', port=8000, debug=True)
```

#### Step 3: Test Backend
```bash
python server.py

# In another terminal:
curl http://localhost:8000/api/health
```

---

## Dashboard Frontend Setup

### Step 1: Create React Application

```bash
cd /Users/justinbravo/Desktop/Coding/School/Cloud_Computing/tj_omnet/dashboard

# Create React app (choose one):
# Option 1: Using Vite (faster, recommended)
npm create vite@latest frontend -- --template react
cd frontend
npm install

# Option 2: Using Create React App
npx create-react-app frontend
cd frontend
```

### Step 2: Install Frontend Dependencies

```bash
# UI and visualization libraries
npm install @mui/material @emotion/react @emotion/styled
npm install cytoscape cytoscape-dom-node
npm install recharts  # For charts/graphs
npm install axios socket.io-client  # For API communication

# Development tools
npm install --save-dev @vitejs/plugin-react
```

### Step 3: Configure API Connection

Create `src/services/api.js`:
```javascript
import axios from 'axios';

const API_BASE_URL = 'http://localhost:8000/api';

const apiClient = axios.create({
    baseURL: API_BASE_URL,
    headers: {
        'Content-Type': 'application/json',
    },
});

export const healthCheck = () => apiClient.get('/health');

// Slice API
export const getSlices = () => apiClient.get('/slices');
export const createSlice = (config) => apiClient.post('/slices', config);
export const deleteSlice = (id) => apiClient.delete(`/slices/${id}`);

// Flow API
export const getFlows = () => apiClient.get('/flows');
export const installFlow = (rule) => apiClient.post('/flows', rule);
export const removeFlow = (id) => apiClient.delete(`/flows/${id}`);

export default apiClient;
```

### Step 4: Create WebSocket Service

Create `src/services/websocket.js`:
```javascript
import io from 'socket.io-client';

const WS_URL = 'http://localhost:8000';

let socket = null;

export const connectWebSocket = (onMessage) => {
    socket = io(WS_URL);

    socket.on('connect', () => {
        console.log('Connected to WebSocket');
    });

    socket.on('metrics', (data) => {
        onMessage(data);
    });

    return socket;
};

export const disconnectWebSocket = () => {
    if (socket) {
        socket.disconnect();
    }
};

export default { connectWebSocket, disconnectWebSocket };
```

### Step 5: Create Basic Dashboard Layout

Update `src/App.jsx`:
```javascript
import React, { useState, useEffect } from 'react';
import { Container, AppBar, Toolbar, Typography } from '@mui/material';
import { healthCheck } from './services/api';
import './App.css';

function App() {
    const [backendStatus, setBackendStatus] = useState('checking...');

    useEffect(() => {
        healthCheck()
            .then(() => setBackendStatus('connected'))
            .catch(() => setBackendStatus('disconnected'));
    }, []);

    return (
        <div className="App">
            <AppBar position="static">
                <Toolbar>
                    <Typography variant="h6">
                        Cloud SDN Dashboard
                    </Typography>
                    <Typography variant="body2" style={{ marginLeft: 'auto' }}>
                        Backend: {backendStatus}
                    </Typography>
                </Toolbar>
            </AppBar>
            <Container maxWidth="xl" style={{ marginTop: '2rem' }}>
                <Typography variant="h4">
                    Welcome to SDN Management Dashboard
                </Typography>
                <Typography variant="body1" style={{ marginTop: '1rem' }}>
                    Network Slicing and Flow Provisioning Interface
                </Typography>
            </Container>
        </div>
    );
}

export default App;
```

### Step 6: Start Frontend Development Server

```bash
npm run dev  # If using Vite
# or
npm start    # If using Create React App

# Dashboard should open at http://localhost:3000
```

---

## Verification and Testing

### Complete System Test

#### Terminal 1: Start Backend
```bash
cd dashboard/backend
npm run dev  # Node.js
# or
python server.py  # Python
```

#### Terminal 2: Start Frontend
```bash
cd dashboard/frontend
npm run dev
```

#### Terminal 3: Run OMNeT++ Sample
```bash
cd samples/aloha
./aloha -u Qtenv  # GUI mode
# or
./aloha -u Cmdenv  # Command-line mode
```

### Verification Checklist

- [ ] OMNeT++ GUI opens successfully (`omnetpp`)
- [ ] Sample simulation runs (`samples/aloha`)
- [ ] INET examples execute without errors
- [ ] OpenFlow example runs (if installed)
- [ ] Backend responds to `/api/health` endpoint
- [ ] Frontend loads at http://localhost:3000
- [ ] Frontend shows "Backend: connected" status
- [ ] Browser console has no errors

---

## Troubleshooting

### OMNeT++ Issues

**Problem:** `./configure` fails with "Qt not found"
```bash
# Solution: Install Qt5 development packages
sudo apt-get install qt5-default libqt5opengl5-dev  # Ubuntu
brew install qt@5  # macOS
```

**Problem:** Compilation errors about C++17
```bash
# Solution: Ensure modern compiler
g++ --version  # Should be 9.0+
# Update if necessary
```

**Problem:** "Cannot find INET" when building SDN framework
```bash
# Solution: Export INET_ROOT
export INET_ROOT=/path/to/inet
# Add to ~/.bashrc to persist
```

### Dashboard Issues

**Problem:** `npm install` fails with permission errors
```bash
# Solution: Fix npm permissions (don't use sudo!)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
```

**Problem:** Backend starts but frontend can't connect
```bash
# Solution: Check CORS settings in backend
# Ensure backend allows http://localhost:3000
# Check backend is running on correct port (8000)
```

**Problem:** WebSocket connection fails
```bash
# Solution: Verify WebSocket server is initialized
# Check browser console for connection errors
# Ensure firewall allows connections
```

### Integration Issues

**Problem:** Dashboard can't communicate with OMNeT++
```bash
# This is expected at this stage!
# Integration will be implemented in Week 2-3
# For now, backend can return mock data
```

---

## Next Steps

After completing this setup, you should:

1. **Read the [Technical Plan](../TECHNICAL_PLAN.md)** to understand the project phases
2. **Familiarize with OMNeT++:**
   - Complete tictoc tutorial: `doc/tictoc-tutorial/`
   - Read Simulation Manual: `doc/manual/`
3. **Explore INET examples:**
   - Browse `inet/examples/` directory
   - Run different scenarios to understand network models
4. **Study OpenFlow basics:**
   - Read OpenFlow specification v1.3
   - Understand flow table structure
   - Review OpenFlowOMNeTSuite examples
5. **Design your first topology:**
   - Create simple NED file with 2 switches and 4 hosts
   - Configure basic connectivity
   - Test with ping traffic
6. **Prototype dashboard features:**
   - Create slice list component
   - Mock API responses
   - Design UI layout

---

## Environment Summary

After successful setup, your environment should have:

```
✅ OMNeT++ 7.0.0 (compiled and tested)
✅ INET Framework 4.5.x (compiled and tested)
✅ OpenFlowOMNeTSuite or SDN4CoRE (compiled and tested)
✅ Node.js 18+ or Python 3.9+ (backend ready)
✅ React development environment (frontend ready)
✅ All dependencies installed
✅ Sample simulations running
✅ Backend API responding
✅ Frontend loading in browser
```

**Total Setup Time:** 2-4 hours (depending on internet speed and hardware)

---

## Additional Resources

### Documentation
- **OMNeT++ Manual:** `doc/manual/index.html`
- **INET Guide:** https://inet.omnetpp.org/docs/users-guide/
- **OpenFlow Spec:** https://opennetworking.org/software-defined-standards/specifications/

### Tutorials
- **OMNeT++ TicToc:** `doc/tictoc-tutorial/`
- **INET Tutorials:** https://inet.omnetpp.org/docs/tutorials/
- **React Docs:** https://react.dev/learn

### Community
- **OMNeT++ Google Group:** https://groups.google.com/g/omnetpp
- **INET Mailing List:** https://groups.google.com/g/inet-users
- **Stack Overflow:** Tag [omnet++]

---

## Contact

If you encounter issues not covered here:
1. Check [Troubleshooting](#troubleshooting) section
2. Search OMNeT++ Google Group for similar issues
3. Contact team members: [your-email@university.edu]

---

**Document Version:** 1.0
**Last Updated:** [Date]
**Next Review:** After Week 1 completion

[Back to Documentation Index](../TECHNICAL_PLAN.md)
