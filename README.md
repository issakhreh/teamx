# .github/workflows/go-ci.yml
name: 🧪 Go CI Pipeli

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Go
      uses: actions/setup-go@v4
      with:
        go-version: '1.21'

    - name: Install dependencies
      run: go mod tidy

    - name: Run linter
      uses: golangci/golangci-lint-action@v3
      with:
        version: latest

    - name: Run tests
      run: go test -v ./... -coverprofile=coverage.out

    - name: Upload test results
      uses: actions/upload-artifact@v3
      with:
        name: go-test-results
        path: coverage.out
