# Blazor in .NET 8: Sample 9

Let's delve into a more advanced scenario where we'll use Blazor to create a real-time data visualization component.

Example: Real-time Stock Price Chart

Features Demonstrated:

Timers: Updating data and UI at regular intervals.
External Data Sources: Fetching data from an API or a simulated stock ticker.
Data Visualization: (Optional) Using a charting library to render an interactive chart.
Blazor Lifecycle Methods: Managing component updates and cleanup.
Setup

Project: Create a new Blazor Server project (or reuse an existing one). You can name it "RealtimeCharts".
Optional Charting Library: If you want a fully-featured chart, explore integrating a JavaScript charting library like Chart.js (Refer to the earlier "Interactive Chart" example for guidance)
Models

StockData.cs: (Inside a Models folder)

C#
namespace RealtimeCharts.Models;

public class StockData
{
    public string Symbol { get; set; }
    public decimal Price { get; set; }
    public DateTime Timestamp { get; set; }
}
Usa el código con precaución.
Services (Optional)

StockTickerService.cs (Inside a Services folder - for simulation)

C#
namespace RealtimeCharts.Services;

public class StockTickerService
{
    private readonly Random _random = new Random();
    private Timer _timer;
    private List<StockData> _data = new List<StockData>(); // Holds the data

    public event Action OnDataUpdate; 

    public StockTickerService()
    {
        // Add some initial sample data
        ... 
    }

    public void Start() 
    {
        _timer = new Timer(UpdateData, null, 1000, 1000); // Update every second
    }

    public void Stop()
    {
        _timer?.Dispose(); 
        _timer = null;
    }

    public List<StockData> GetData() => _data.ToList(); 

    private void UpdateData()
    {
        // Simulate price changes (replace this with an actual API call if needed)
        ... 
        OnDataUpdate?.Invoke(); 
    }
}
Usa el código con precaución.
Components

StockChart.razor

Razor CSHTML
@inject StockTickerService TickerService  
@implements IDisposable

<h3>Live Stock Chart</h3>

@* Replace this with charting library code if using one *@
<div class="chart-container">
    @foreach (var dataPoint in TickerService.GetData())
    {
        <div class="data-point" style="height: @CalculateHeight(dataPoint.Price)px">
            @dataPoint.Price
        </div>
    }
</div>

@code {
    protected override void OnInitialized()
    {
        TickerService.OnDataUpdate += StateHasChanged;
        TickerService.Start(); 
    }

    public void Dispose()
    {
        TickerService.OnDataUpdate -= StateHasChanged;
        TickerService.Stop(); // Stop when the component is disposed
    }

    // Helper for basic visualization
    private int CalculateHeight(decimal price) 
    {
        // Adjust as needed 
        return (int)(price * 5); 
    }        
}
Usa el código con precaución.

