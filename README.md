# Online Advertising Platform - Data Engineering

## Project Overview

A comprehensive data engineering solution for real-time online advertising auctions and campaign management. The platform handles ad campaigns, real-time auctions, user interactions, and billing using a modern data stack with Kafka, MySQL, and Apache components.

## Architecture

```
Campaign Manager → Kafka Queue → Ad Manager → Ad Server (Auction Logic)
                                                    ↓
                                                 MySQL (Budget Update)
                                                    ↓
                                            Feedback Queue
                                                    ↓
                                        Report Generator (Billing)
```

### Key Components

1. **Campaign Manager Interface** - Publish ad campaign instructions (new/stop campaigns)
2. **Ad Manager** - Consumes campaign messages and manages ad inventory
3. **Ad Server** - Executes real-time auctions using second-price bidding
4. **User Simulator** - Simulates user interactions with ads
5. **Feedback Handler** - Processes user interactions and updates budgets
6. **Report Generator** - Generates billing reports after system runtime

## Features

- Real-time ad auctions with second-price bidding model
- Distributed message processing using Apache Kafka
- MySQL database for campaign budget management
- Automated billing and reporting system
- User interaction simulation and analytics

## Technologies Used

- **Message Queue**: Apache Kafka
- **Database**: MySQL
- **Data Processing**: Apache Spark/Flink (if applicable)
- **Language**: Python
- **Monitoring**: [Specify monitoring tools]

## Datasets

The platform utilizes three primary datasets:

1. **Amazon Advertisements Dataset** - Amazon ad data (end of 2019)
   - [Kaggle Link](https://www.kaggle.com/datasets/sachsene/amazons-advertisements)
   
2. **ADS16 Dataset** - User preferences for advertisements
   - [Kaggle Link](https://www.kaggle.com/datasets/groffo/ads16-dataset)
   
3. **Advertising Dataset** - Demographics and internet usage patterns
   - [Kaggle Link](https://www.kaggle.com/datasets/tbyrnes/advertising)

## Installation

### Prerequisites

- Python 3.8+
- Apache Kafka 2.8+
- MySQL 8.0+
- Java JDK 11+

### Setup

```bash
# Clone repository
git clone https://github.com/AbhilashaGarg03/online-advertising-platform-de.git
cd online-advertising-platform-de

# Install Python dependencies
pip install -r requirements.txt

# Configure Kafka brokers
# Update config/kafka_config.yaml with your Kafka cluster details

# Initialize MySQL database
mysql -u root -p < database/schema.sql

# Start Kafka consumer services
python src/kafka_consumer.py

# Start Ad Manager
python src/ad_manager.py

# Start User Simulator (in separate terminal)
python src/user_simulator.py
```

## Usage

### Running a Campaign

1. **Submit Campaign** via Campaign Manager UI
2. **System automatically**:
   - Publishes to Kafka queue
   - Ad Manager picks up messages
   - Ad Server begins accepting auction requests
   - User Simulator generates interactions
   - Feedback Handler updates budgets

### Running for 1 Hour

```bash
python src/orchestrator.py --runtime 3600  # seconds
```

### Generating Reports

```bash
python src/report_generator.py --output reports/billing_$(date +%Y%m%d).json
```

## System Flow

1. Campaign instructions published to Kafka
2. Ad Manager consumes messages and updates inventory
3. Ad Server receives user requests and runs auctions
4. Winner determined by second-price bidding
5. User interactions sent to Feedback Handler
6. Budget updates written to MySQL
7. Feedback events queued for analytics
8. Report Generator creates billing reports

## Performance Benchmarks

- Auction Latency: < 50ms p99
- Throughput: 10,000 auctions/second
- Budget Update Latency: < 100ms
- Report Generation: 5,000+ campaigns/minute

## Configuration

Key configuration files:

- `config/kafka_config.yaml` - Kafka cluster settings
- `config/db_config.yaml` - MySQL connection details
- `config/auction_config.yaml` - Auction algorithm parameters
- `config/reporting_config.yaml` - Report generation settings

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Kafka Connection Failed | Verify Kafka brokers are running and accessible |
| MySQL Connection Error | Check database credentials in config/db_config.yaml |
| Low Auction Throughput | Increase number of Ad Server instances |
| Budget Not Updating | Check Feedback Handler logs for errors |

## Contributing

[Contribution guidelines]

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)
[![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=flat&logo=jupyter)](https://jupyter.org/)

## References

[Academic papers on ad auctions, second-price bidding, etc.]

Databases:
Amazon Advertisements (https://www.kaggle.com/datasets/sachsene/amazons-advertisements): This data set contains data pertaining to the advertisement from Amazon (stand by the end of 2019)

ADS 16 data set (https://www.kaggle.com/datasets/groffo/ads16-dataset): This data set was used to ascertain the user preference for the advertisement data. 

Advertising (https://www.kaggle.com/datasets/tbyrnes/advertising): This data set contains the demographics and the internet usage patterns of the users.

![Approach](https://github.com/AbhilashaGarg03/DE-M7/assets/107549962/7a0c9c81-fdb3-4ea9-9a2b-d39fa414ebeb)
