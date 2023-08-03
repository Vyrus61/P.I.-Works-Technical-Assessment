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
Expected input
````
```
exhibitA-input.csv(which is written in code)
```
````
Expected output
````
```
 --------------------------------------
 | DISTINCT_PLAY_COUNT | CLIENT_COUNT |
 --------------------------------------
 | 281                 | 1            |
 --------------------------------------
 | 293                 | 1            |
 --------------------------------------
 | 298                 | 1            |
 --------------------------------------
 | 299                 | 1            |
 --------------------------------------
 | 300                 | 2            |
 --------------------------------------
 | 301                 | 2            |
 --------------------------------------
 | 302                 | 2            |
 --------------------------------------
 | 303                 | 2            |
 --------------------------------------
 | 304                 | 1            |
 --------------------------------------
 | 305                 | 5            |
 --------------------------------------
 | 306                 | 5            |
 --------------------------------------
 | 307                 | 3            |
 --------------------------------------
 | 308                 | 5            |
 --------------------------------------
 | 309                 | 3            |
 --------------------------------------
 | 310                 | 4            |
 --------------------------------------
 | 311                 | 1            |
 --------------------------------------
 | 312                 | 3            |
 --------------------------------------
 | 313                 | 3            |
 --------------------------------------
 | 314                 | 4            |
 --------------------------------------
 | 315                 | 6            |
 --------------------------------------
 | 316                 | 5            |
 --------------------------------------
 | 317                 | 9            |
 --------------------------------------
 | 318                 | 9            |
 --------------------------------------
 | 319                 | 10           |
 --------------------------------------
 | 320                 | 6            |
 --------------------------------------
 | 321                 | 13           |
 --------------------------------------
 | 322                 | 14           |
 --------------------------------------
 | 323                 | 11           |
 --------------------------------------
 | 324                 | 8            |
 --------------------------------------
 | 325                 | 7            |
 --------------------------------------
 | 326                 | 9            |
 --------------------------------------
 | 327                 | 11           |
 --------------------------------------
 | 328                 | 16           |
 --------------------------------------
 | 329                 | 17           |
 --------------------------------------
 | 330                 | 19           |
 --------------------------------------
 | 331                 | 14           |
 --------------------------------------
 | 332                 | 13           |
 --------------------------------------
 | 333                 | 16           |
 --------------------------------------
 | 334                 | 21           |
 --------------------------------------
 | 335                 | 22           |
 --------------------------------------
 | 336                 | 24           |
 --------------------------------------
 | 337                 | 24           |
 --------------------------------------
 | 338                 | 26           |
 --------------------------------------
 | 339                 | 16           |
 --------------------------------------
 | 340                 | 14           |
 --------------------------------------
 | 341                 | 18           |
 --------------------------------------
 | 342                 | 19           |
 --------------------------------------
 | 343                 | 21           |
 --------------------------------------
 | 344                 | 24           |
 --------------------------------------
 | 345                 | 18           |
 --------------------------------------
 | 346                 | 22           |
 --------------------------------------
 | 347                 | 35           |
 --------------------------------------
 | 348                 | 26           |
 --------------------------------------
 | 349                 | 18           |
 --------------------------------------
 | 350                 | 27           |
 --------------------------------------
 | 351                 | 16           |
 --------------------------------------
 | 352                 | 29           |
 --------------------------------------
 | 353                 | 16           |
 --------------------------------------
 | 354                 | 29           |
 --------------------------------------
 | 355                 | 19           |
 --------------------------------------
 | 356                 | 14           |
 --------------------------------------
 | 357                 | 17           |
 --------------------------------------
 | 358                 | 27           |
 --------------------------------------
 | 359                 | 13           |
 --------------------------------------
 | 360                 | 18           |
 --------------------------------------
 | 361                 | 13           |
 --------------------------------------
 | 362                 | 22           |
 --------------------------------------
 | 363                 | 8            |
 --------------------------------------
 | 364                 | 14           |
 --------------------------------------
 | 365                 | 8            |
 --------------------------------------
 | 366                 | 4            |
 --------------------------------------
 | 367                 | 9            |
 --------------------------------------
 | 368                 | 10           |
 --------------------------------------
 | 369                 | 7            |
 --------------------------------------
 | 370                 | 5            |
 --------------------------------------
 | 371                 | 4            |
 --------------------------------------
 | 372                 | 4            |
 --------------------------------------
 | 373                 | 8            |
 --------------------------------------
 | 374                 | 2            |
 --------------------------------------
 | 375                 | 5            |
 --------------------------------------
 | 376                 | 4            |
 --------------------------------------
 | 377                 | 8            |
 --------------------------------------
 | 378                 | 3            |
 --------------------------------------
 | 379                 | 2            |
 --------------------------------------
 | 381                 | 3            |
 --------------------------------------
 | 382                 | 4            |
 --------------------------------------
 | 383                 | 1            |
 --------------------------------------
 | 384                 | 2            |
 --------------------------------------
 | 385                 | 2            |
 --------------------------------------
 | 386                 | 1            |
 --------------------------------------
 | 387                 | 3            |
 --------------------------------------
 | 388                 | 1            |
 --------------------------------------
 | 389                 | 3            |
 --------------------------------------
 | 390                 | 1            |
 --------------------------------------
 | 391                 | 1            |
 --------------------------------------
 | 392                 | 1            |
 --------------------------------------
 | 393                 | 1            |
 --------------------------------------

 Count: 97


....exe (process 112852) exited with code 0.
To automatically close the console when debugging stops, enable Tools->Options->Debugging->Automatically close the console when debugging stops.
Press any key to close this window . . .

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
