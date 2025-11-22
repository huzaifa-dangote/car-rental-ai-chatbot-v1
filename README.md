# AI Assistant for Exclusive Car Rentals

A conversational AI chatbot built with OpenAI GPT-4 and Gradio that helps customers find and book vehicles from Exclusive Car Rentals. The chatbot can answer questions about available vehicles, provide pricing information for different service types, and guide customers through the booking process.

## Features

- 🤖 **AI-Powered Chatbot**: Uses OpenAI GPT-4.1-mini for natural language conversations
- 🚗 **Vehicle Information**: Access to 13 different vehicles with detailed specifications
- 💰 **Dynamic Pricing**: Real-time price queries based on vehicle and service type
- 💬 **Interactive Interface**: Beautiful Gradio chat interface with dark mode support
- 🗄️ **SQLite Database**: Stores vehicle information and pricing data locally
- 🔧 **Function Calling**: Uses OpenAI function calling to query vehicle prices from the database

## Requirements

### Python Version
- Python 3.12+ (or compatible version)

### Dependencies
- `openai` - OpenAI API client
- `gradio` (6.0.0+) - Web interface framework
- `python-dotenv` - Environment variable management
- `sqlite3` - Built-in Python module for database operations

## Installation

### 1. Clone the Repository
```bash
git clone <repository-url>
cd car-rental-ai-chatbot-v1
```

### 2. Create a Virtual Environment (Recommended)
```bash
python3 -m venv .venv
source .venv/bin/activate  # On macOS/Linux
# or
.venv\Scripts\activate  # On Windows
```

### 3. Install Dependencies
```bash
pip install python-dotenv openai gradio
```

## Setup

### 1. OpenAI API Key

Create a `.env` file in the project root directory:

```bash
touch .env
```

Add your OpenAI API key to the `.env` file:

```
OPENAI_API_KEY=sk-your-api-key-here
```

**Note**: You can obtain an API key from [OpenAI's website](https://platform.openai.com/api-keys).

### 2. Database Setup

The application uses two SQLite databases:
- `vehicle.db` - Stores vehicle information (name, type, seats, features, etc.)
- `vehicle_prices.db` - Stores pricing information for different service types

The databases are automatically created and populated when you run the notebook cells. The database setup is handled in:
- **Cell 7**: Creates the database tables
- **Cell 8**: Populates the databases with vehicle and pricing data

### 3. Running the Application

Open the Jupyter notebook:

```bash
jupyter notebook app.ipynb
```

Or if using VS Code, simply open `app.ipynb` and run the cells in order.

**Important**: Make sure to:
1. Run cells 1-2 to import dependencies and initialize the OpenAI client
2. Run cell 7 to create the database tables
3. Run cell 8 to populate the databases (only needed once)
4. Run cell 15 to launch the Gradio interface

## Usage

### Starting the Chatbot

1. Run all cells in the notebook sequentially
2. The Gradio interface will launch automatically
3. Access the chatbot at the local URL shown (typically `http://127.0.0.1:7864`)

### Interacting with the Chatbot

The chatbot can help with:
- **Vehicle inquiries**: Ask about available vehicles, their features, seating capacity, etc.
- **Pricing information**: Ask about rental prices for specific vehicles and service types
- **Booking guidance**: Get help understanding the booking process

### Example Queries

- "What vehicles do you have available?"
- "How much does it cost to rent a Toyota Land Cruiser for a full day?"
- "I need a vehicle for 15 people, what do you recommend?"
- "What's the price for airport pickup with a Toyota Camry?"

## Available Vehicles

The system includes 13 vehicles:

1. Toyota Coaster (30 seats)
2. Mercedes G-Wagon (5 seats)
3. Toyota Hiace 2018 (18 seats)
4. Toyota Land Cruiser 2020 (7 seats)
5. Mercedes Sprinter (15 seats)
6. Lexus GX 460 (7 seats)
7. Toyota Hilux (5 seats)
8. Lexus LX 470 (5 seats)
9. Toyota Camry (5 seats)
10. Toyota Prado (7 seats)
11. Hyundai H1 (11 seats)
12. Toyota Land Cruiser (7 seats)
13. Toyota Hiace (18 seats)

## Service Types

Each vehicle supports 5 service types:

1. **`airport_pickup`** - Airport pickup service (default if not specified)
2. **`hourly_rate`** - Hourly rental
3. **`full_day_8hr`** - 8-hour full day rental
4. **`full_24hr`** - 24-hour rental
5. **`border_states_roundtrip`** - Round trip to border states

## Project Structure

```
car-rental-ai-chatbot-v1/
├── app.ipynb              # Main Jupyter notebook with the application
├── README.md              # This file
├── .env                   # Environment variables (create this file)
├── vehicle.db             # SQLite database for vehicle information
├── vehicle_prices.db      # SQLite database for pricing information
└── .venv/                 # Virtual environment (created during setup)
```

## Technical Details

### Model Configuration
- **Model**: `gpt-4.1-mini` (OpenAI)
- **Function Calling**: Enabled for dynamic price queries

### Database Schema

#### `vehicles` Table
- `id` (UUID, PRIMARY KEY)
- `name` (TEXT)
- `type` (TEXT)
- `image` (TEXT)
- `seats` (INT4)
- `transmission` (TEXT)
- `fuel` (TEXT)
- `features` (TEXT)
- `available` (BOOL)
- `created_at` (TIMESTAMPTZ)
- `updated_at` (TIMESTAMPTZ)
- `year` (INT4)
- `popularity_status` (TEXT)

#### `vehicle_prices` Table
- `id` (UUID, PRIMARY KEY)
- `vehicle_id` (UUID, FOREIGN KEY)
- `service_type` (TEXT)
- `price` (NUMERIC)
- `is_available` (BOOL)
- `created_at` (TIMESTAMPTZ)
- `updated_at` (TIMESTAMPTZ)

### Function Calling

The chatbot uses OpenAI's function calling feature to query vehicle prices:
- Function name: `get_vehicle_price`
- Parameters: `vehicle_name` (string), `service_type` (string)
- Returns: Price information from the database

## Configuration

### System Message
The chatbot is configured with a system message that:
- Identifies it as an assistant for Exclusive Car Rentals
- Provides context about the booking process
- Lists available vehicles and service types
- Sets default behavior (uses `airport_pickup` if service type not specified)

### Dark Mode
The application includes JavaScript code to force dark mode (defined in Cell 3). To use it, you can modify the launch command in Cell 15:

```python
gr.ChatInterface(fn=chat, js=force_dark_mode).launch()
```

Or use Gradio's built-in theme:

```python
gr.ChatInterface(fn=chat, theme=gr.themes.Monochrome()).launch()
```

## Troubleshooting

### OpenAI API Key Issues
- Ensure your `.env` file exists and contains `OPENAI_API_KEY=your-key-here`
- Verify the API key is valid and has sufficient credits
- Check that `load_dotenv(override=True)` is called before initializing the OpenAI client

### Database Errors
- Make sure cells 7 and 8 have been executed to create and populate the databases
- If you get duplicate key errors, the databases may already be populated (this is fine)

### Gradio Launch Issues
- Ensure Gradio 6.0.0+ is installed: `pip install --upgrade gradio`
- Check that no other application is using the default port (7864)
- Try specifying a different port: `.launch(server_port=7865)`

## Notes

- The application uses SQLite databases that are created locally in the project directory
- Vehicle data and pricing information are pre-populated in the notebook
- The chatbot is designed for Exclusive Car Rentals (https://www.ecr.ng)
- The booking process involves: selecting service → selecting vehicle → entering details → contact by customer service

## Contact

For questions or support, please contact huzaifa@aixcelera.com
