# JsonSaver
This is an easy way to store JSON array data to .json files.
### How to use
The default namespace for JSONSaver is just simply JSONSaver

The namespace for the interface ISaveable is JSONSaver.Types
If you want, the namespace for saving data, converting it, etc is JSONSaver.Data

Here is an example (brought directly from JsonDBTesting)
```
using System;
using JSONSaver;
using JSONSaver.Types;

namespace JsonDBTesting
{
	class Program
	{
		static void Main(string[] args)
		{
			var db = new JSONDatabase<TestClass, string>("C:/Users/slimc/Desktop/data.json", a => new TestClass(), formatting: Newtonsoft.Json.Formatting.Indented);

			var value = db.GetOrAdd("CoolGuy1234");

			db.SaveData();

			Console.WriteLine($"Edit the json file. Will print password after you press enter. Current password is {value.Password}");
			Console.ReadLine();

			db.Reload();

			var newvalue = db.GetOrAdd("CoolGuy1234"); // The instance is new

			Console.WriteLine($"New password is: {newvalue.Password}");

			Console.WriteLine("Do you want to change the password (y/n)?");

			var k = Console.ReadLine().ToLower();
			
			if(k == "y")
			{
				Console.WriteLine("What do you want to replace the password with?");

				newvalue.Password = Console.ReadLine();
				db.SaveData();

				Console.WriteLine("Changed password");
				
			}

			Console.ReadKey();
		}
	}

	class TestClass : ISaveable<String>
	{
		public string Username { get; set; }
		public string Password { get; set; }
		string ISaveable<string>.Key { get => Username; set => Username = value; }
	}
}

/*
Json Database file at end:

[
  {
    "Username": "CoolGuy1234",
    "Password": "password1234"
  }
]
*/

/*
Console output at end:
Edit the json file. Will print password after you press enter. Current password is (null)

New password is: Hahaha
Do you want to change the password (y/n)?
y
What do you want to replace the password with?
password1234
Changed password
*/
```
