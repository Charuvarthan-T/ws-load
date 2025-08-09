# Godo : WebSocket Load Testing Tool

A high-performance, native Go CLI tool for WebSocket load testing that leverages Go's efficient goroutines and channels for highly concurrent load generation.

## Features

- **Real-time bi-directional WebSocket communication testing**
- **High-performance load generation** with minimal resource consumption
- **Comprehensive performance metrics** (RPS, Latency, Throughput, Percentiles)
- **Test history tracking** with automatic result persistence
- **Visual trend analysis** with ASCII charts for key metrics
- **Human-first CLI design** with progress indicators and clear feedback
- **Robust error handling** and detailed logging
- **JSON message support** with automatic validation
- **Flexible test configuration** with duration, connections, and loop controls

##  Developers

| [<img src="https://github.com/SaiNivedh26.png" width="100px;"><br><sub><b>Sai Nivedh</b></sub>](https://github.com/SaiNivedh26) | [<img src="https://github.com/charuvarthan.png" width="100px;"><br><sub><b>Charuvarthan</b></sub>](https://github.com/charuvarthan) | [<img src="https://github.com/sajeevsenthil.png" width="100px;"><br><sub><b>Sajeev Senthil</b></sub>](https://github.com/sajeevsenthil) |
| :---: | :---: | :---: |



<br>
<br>
<br>


------------------------------------------------------------------------------
## Installation

### Prerequisites

- Go 1.24 or later

### Build from Source

```bash
# Clone the repository
git clone <repository-url>
cd ws-load

# Build the binary
go build -o ws-load

# Make it executable (Linux/macOS)
chmod +x ws-load

# Move to a directory in your PATH (optional)
sudo mv ws-load /usr/local/bin/
```

### Using go install

```bash
go install github.com/your-username/ws-load@latest
```

## Usage

### Basic Usage

```bash
# Run a simple test against echo.websocket.org
ws-load test -u ws://echo.websocket.org

# Run with custom parameters
ws-load test -u ws://localhost:8080/ws -d 30s -c 50 -m "Hello, WebSocket!"

# Run with JSON message
ws-load test -u ws://localhost:8080/ws --message '{"type":"ping","data":"test"}'
```

### Command Options

#### Global Options

- `-v, --verbose`: Enable verbose output for detailed logging

#### Test Command Options

- `-u, --url`: WebSocket endpoint URL (required)
  - Examples: `ws://echo.websocket.org`, `wss://secure.example.com/ws`
  - If no scheme is provided, `ws://` is automatically added

- `-d, --duration`: Test duration (default: 30s)
  - Examples: `10s`, `5m`, `1h`, `2h30m`

- `-c, --connections`: Number of concurrent connections (default: 10)
  - Range: 1 to any positive integer

- `-m, --message`: Message to send (default: "Hello, WebSocket!")
  - Supports plain text and JSON
  - JSON messages are automatically validated

- `-l, --loop`: Number of times to send message per connection (default: 1)
  - Range: 1 to any positive integer

### Examples

#### Basic Load Test

```bash
# Test with 100 concurrent connections for 1 minute
ws-load test -u ws://echo.websocket.org -d 1m -c 100
```

#### High-Intensity Test

```bash
# Send JSON messages with high concurrency
ws-load test \
  -u ws://localhost:8080/ws \
  -d 5m \
  -c 500 \
  -m '{"action":"ping","timestamp":1234567890}' \
  -l 10
```

#### Verbose Testing

```bash
# Enable verbose output for debugging
ws-load test -u ws://localhost:8080/ws -d 30s -c 10 -v
```

#### Configuration Check

```bash
# View current configuration
ws-load config --show
```

## Performance Metrics

The tool provides comprehensive performance metrics:

### Request Metrics
- **Total Requests**: Total number of requests sent
- **Successful Requests**: Number of successfully processed requests
- **Failed Requests**: Number of failed requests with error details
- **Requests per Second (RPS)**: Rate of request processing

### Latency Metrics
- **Average Latency**: Mean response time
- **P50 Latency**: Median response time (50th percentile)
- **Latency Distribution**: Detailed latency statistics

### Throughput Metrics
- **Data Throughput**: Bytes transferred per second
- **Bytes Sent**: Total data sent
- **Bytes Received**: Total data received

### Error Analysis
- **Error Counts**: Breakdown of different error types
- **Status Codes**: Distribution of HTTP/WebSocket status codes

## Output Format

The tool provides clear, formatted output:

```
╔══════════════════════════════════════════════════════════════╗
║                    WebSocket Load Test Results              ║
╚══════════════════════════════════════════════════════════════╝

Test Configuration:
  URL:         ws://echo.websocket.org
  Duration:    30s
  Connections: 50
  Message:     Hello, WebSocket!
  Loop Count:  1

Performance Metrics:
  Total Requests:     1500
  Successful:         1498 (99.9%)
  Failed:             2 (0.1%)
  Requests/sec:       50.00
  Avg Latency:        45.2ms
  P50 Latency:        42.1ms
  Throughput:         1.2 KB/sec
  Bytes Sent:         36 KB
  Bytes Received:     36 KB

Error Summary:
  connection_failed_23: 1
  send_failed_45_2: 1

Test completed in 30s
```

## Architecture

### Core Components

1. **CLI Interface**: Built with `github.com/jessevdk/go-flags` for robust argument parsing
2. **WebSocket Client**: Uses `github.com/lxzan/gws` for high-performance WebSocket operations
3. **Metrics Collection**: Leverages `github.com/hashicorp/go-metrics` for comprehensive metrics
4. **Progress Tracking**: Implements `github.com/schollz/progressbar/v3` for user feedback

### Concurrency Model

- **Goroutine-based**: Each connection runs in its own goroutine
- **Connection Pool**: Manages concurrent connections efficiently
- **Channel-based Communication**: Uses Go channels for coordination
- **Context Cancellation**: Graceful shutdown with context support

### Performance Optimizations

- **Memory-efficient**: Minimal memory allocation during testing
- **Low-latency**: Optimized for high-frequency message sending
- **Resource Management**: Proper cleanup of connections and resources
- **Non-blocking Operations**: Asynchronous message handling

## Testing

### Unit Tests

```bash
# Run all tests
go test

# Run tests with verbose output
go test -v

# Run specific test
go test -run TestValidateTestOptions

# Run tests with coverage
go test -cover
```

### Integration Tests

```bash
# Test against echo.websocket.org
ws-load test -u ws://echo.websocket.org -d 10s -c 10

# Test with JSON messages
ws-load test -u ws://echo.websocket.org -d 10s -c 5 -m '{"test":"data"}'
```

## Error Handling

The tool provides comprehensive error handling:

### Connection Errors
- Network connectivity issues
- Invalid WebSocket URLs
- Server unavailability
- Timeout handling

### Message Errors
- Invalid JSON format
- Message sending failures
- Response parsing errors

### Configuration Errors
- Invalid duration formats
- Invalid connection counts
- Missing required parameters

## Best Practices

### Test Planning
1. **Start Small**: Begin with low connection counts and short durations
2. **Gradual Scaling**: Increase load gradually to identify breaking points
3. **Monitor Resources**: Watch system resources during high-load tests
4. **Use Realistic Data**: Test with actual message formats and sizes

### Performance Tuning
1. **Connection Limits**: Adjust based on server capacity
2. **Message Frequency**: Balance between RPS and realistic usage
3. **Duration Planning**: Ensure sufficient time for meaningful results
4. **Error Analysis**: Review error patterns for system improvements

### Production Testing
1. **Staging Environment**: Always test in staging before production
2. **Peak Hours**: Test during off-peak hours to minimize impact
3. **Monitoring**: Set up alerts for performance degradation
4. **Documentation**: Record test parameters and results for comparison

## Test History and Visualization

### History Management

The tool automatically saves test results to history for analysis and comparison.

#### View Test History

```bash
# View all test history
ws-load history

# View last 5 tests
ws-load history --limit 5

# Clear all history
ws-load history --clear
```

#### History Output

Each test entry includes:
- Test ID and timestamp
- Test configuration (URL, duration, connections)
- Performance metrics (success rate, RPS, latency, throughput)
- Error summaries (if any)

### Visualization

Create ASCII charts to visualize metric trends across multiple test runs.

#### Available Metrics

```bash
# Visualize success rate trends
ws-load visualize --metric success-rate

# Visualize requests per second
ws-load visualize --metric requests-per-sec

# Visualize average latency
ws-load visualize --metric avg-latency

# Visualize throughput
ws-load visualize --metric throughput
```

#### Visualization Options

- `--metric, -m`: Metric to visualize (success-rate, requests-per-sec, avg-latency, throughput)
- `--limit, -l`: Number of recent tests to include (default: 10)

#### Example Workflow

```bash
# Run multiple tests
ws-load test -u ws://echo.websocket.org -d 10s -c 5
ws-load test -u ws://echo.websocket.org -d 10s -c 10
ws-load test -u ws://echo.websocket.org -d 10s -c 20

# View test history
ws-load history --limit 3

# Visualize performance trends
ws-load visualize --metric requests-per-sec --limit 3
```

#### Sample Visualization Output

```
╔══════════════════════════════════════════════════════════════╗
║                    requests-per-sec Trend Chart             ║
╚══════════════════════════════════════════════════════════════╝

Test ID: 1       2       3
   2.50 |                ████
   2.31 |                ████
   2.12 |                ████
   1.93 |                ████
   1.74 |                ████
   1.55 |                ████
   1.36 |                ████
   1.17 |                ████
   0.98 |                ████
   0.79 |                ████
   0.60 |████    ████    ████
Values:  0.67    0.60    2.50
```

#### History Storage

- History is stored in your home directory as `.ws-load-history.json`
- Each test result is automatically saved upon completion
- History persists across sessions and can be cleared with `ws-load history --clear`

## Troubleshooting

### Common Issues

1. **Connection Refused**
   - Verify the WebSocket server is running
   - Check URL format and port
   - Ensure firewall allows connections

2. **High Latency**
   - Reduce concurrent connections
   - Check network conditions
   - Verify server performance

3. **Memory Issues**
   - Lower connection count
   - Reduce test duration
   - Monitor system resources

4. **Invalid JSON**
   - Validate JSON format
   - Escape special characters
   - Use proper JSON syntax

### Debug Mode

Enable verbose logging for detailed debugging:

```bash
ws-load test -u ws://localhost:8080/ws -d 30s -c 10 -v
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [lxzan/gws](https://github.com/lxzan/gws) - High-performance WebSocket library
- [jessevdk/go-flags](https://github.com/jessevdk/go-flags) - CLI argument parsing
- [hashicorp/go-metrics](https://github.com/hashicorp/go-metrics) - Metrics collection
- [schollz/progressbar](https://github.com/schollz/progressbar) - Progress indicators 

