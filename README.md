# P.I.-Works-Technical-Assessment
P.I. Works Inc Technical Assessment for Ogün Ayaz

Hi Hiring Manager,
I have added the c# code in "Program.cs". I am currently with my family so I was away from my actual computer. That's why ı couldn't properly push the project.

NET Core 3.1.0
Libraries
````
```
CsvHelper==30.0.1
ConsoleTables==2.5.0
```
````

Actual Code
```C#
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using ConsoleTables;
using CsvHelper;
using CsvHelper.Configuration;

class Program
{
    static void Main(string[] args)
    {
        string csvFilePath = "exhibitA-input.csv"; 

        List<PlayData> playDataList = new List<PlayData>();
        using (var reader = new StreamReader(csvFilePath))
        {
            string firstLine = reader.ReadLine();
            char delimiter = DetermineDelimiter(firstLine);
            var csvConfig = new CsvConfiguration(CultureInfo.InvariantCulture)
            {
                Delimiter = delimiter.ToString(),
                HasHeaderRecord = false 
            };

            using (var csv = new CsvReader(reader, csvConfig))
            {
                while (csv.Read())
                {
                    var record = new PlayData
                    {
                        PLAY_ID = Guid.Parse(csv.GetField(0)),
                        SONG_ID = csv.GetField<int>(1),
                        CLIENT_ID = csv.GetField<int>(2)
                    };

                    string playTsString = csv.GetField(3);

                    if (DateTime.TryParseExact(playTsString, "dd/MM/yyyy HH:mm:ss", CultureInfo.InvariantCulture, DateTimeStyles.None, out DateTime playTs))
                    {
                        record.PLAY_TS = playTs;
                    }
                    else if (DateTime.TryParseExact(playTsString, "dd/MM/yyyy", CultureInfo.InvariantCulture, DateTimeStyles.None, out DateTime playDate))
                    {
                        record.PLAY_TS = playDate;
                    }
                    playDataList.Add(record);
                }
            }
        }

        DateTime targetDate = new DateTime(2016, 8, 10);

        var distinctSongPlayCounts = playDataList
            .Where(data => data.PLAY_TS.Date == targetDate)
            .GroupBy(data => data.CLIENT_ID)
            .Select(group => group.Select(data => data.SONG_ID).Distinct().Count())
            .Distinct()
            .OrderBy(count => count)
            .ToList();

        var totalClientsPerDistinctPlayCount = distinctSongPlayCounts
            .ToDictionary(count => count, count => playDataList
                .Where(data => data.PLAY_TS.Date == targetDate)
                .GroupBy(data => data.CLIENT_ID)
                .Count(group => group.Select(data => data.SONG_ID).Distinct().Count() == count));

        var table = new ConsoleTable("DISTINCT_PLAY_COUNT", "CLIENT_COUNT");
        foreach (var entry in totalClientsPerDistinctPlayCount)
        {
            table.AddRow(entry.Key, entry.Value);
        }
        table.Write();
        Console.WriteLine();

    }
    static char DetermineDelimiter(string line)
    {
        if (line.Contains("\t"))
        {
            return '\t';
        }
        else if (line.Contains(","))
        {
            return ',';
        }
        else
        {
            throw new Exception("Couldn't determine delimiter");
        }
    }
}

class PlayData
{
    public Guid PLAY_ID { get; set; }
    public int SONG_ID { get; set; }
    public int CLIENT_ID { get; set; }
    public DateTime PLAY_TS { get; set; }

}
```
