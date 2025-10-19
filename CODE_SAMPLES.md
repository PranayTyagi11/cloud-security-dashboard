# Code Samples

## Terraform Configuration
```hcl
# DynamoDB Table Configuration
resource "aws_dynamodb_table" "metrics" {
  name         = "CloudCostMetrics"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "MetricDate"

  attribute {
    name = "MetricDate"
    type = "S"
  }

  ttl {
    attribute_name = "TTL"
    enabled        = true
  }

  tags = {
    Project = "CloudCostSecurity"
  }
}

# IAM Role for Lambda
resource "aws_iam_role" "lambda_role" {
  name = "cloud-cost-lambda-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "lambda.amazonaws.com"
        }
      }
    ]
  })
}
```

## Terraform Configuration
def get_current_cost():
    """Collect cost data from AWS Cost Explorer"""
    try:
        today = datetime.now()
        first_day = today.replace(day=1)
        
        response = ce.get_cost_and_usage(
            TimePeriod={
                'Start': first_day.strftime('%Y-%m-%d'),
                'End': today.strftime('%Y-%m-%d')
            },
            Granularity='MONTHLY',
            Metrics=['UnblendedCost']
        )
        
        total_cost = Decimal('0.0')
        for result in response['ResultsByTime']:
            cost_amount = result['Total']['UnblendedCost']['Amount']
            total_cost += Decimal(str(cost_amount))
        
        return {
            'total_cost': total_cost,
            'currency': 'USD',
            'period': f"{first_day.strftime('%Y-%m-%d')} to {today.strftime('%Y-%m-%d')}"
        }
        
    except Exception as e:
        logger.error(f"Error getting cost data: {str(e)}")
        return {'total_cost': Decimal('0.0'), 'error': str(e)}

## React Component
const CostChart = ({ data }) => {
  const chartData = {
    labels: data.map(item => item.date),
    datasets: [
      {
        label: 'Daily Cost ($)',
        data: data.map(item => item.cost),
        backgroundColor: 'rgba(37, 99, 235, 0.1)',
        borderColor: '#2563eb',
        borderWidth: 2,
      },
    ],
  };

  return (
    <div className="chart-container">
      <Bar data={chartData} options={chartOptions} />
    </div>
  );
};