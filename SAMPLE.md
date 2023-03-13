# EzApi Sample Code Usage

```
using System;
using System.Linq;
using System.Text;
using System.Collections.Generic;
using Microsoft.SqlServer.Dts.Runtime;
using Microsoft.SqlServer.SSIS.EzAPI;

namespace EzAPIPackageDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            EzPackage package = new EzPackage();
            package.Name = "SSISSamplePackage";

            //Package Variables
            string connection = String.Format("Provider=SQLNCLI11;Integrated Security=SSPI;MarsConn=yes;MARS Connection=True;MultipleActiveResultSets=true;Auto Translate=False;Persist Security Info=True;Data Source={0};Initial Catalog={1}", "TEST_SR", "TEST_DB");
            package.Variables.Add("ConnStringDB", false, "SSISSamplePackageScope", connection);

            //Package Connection
            EzOleDbConnectionManager OLEDBConn = new EzOleDbConnectionManager(package);
            OLEDBConn.CM.SetExpression("ConnectionString", "@[SSISSamplePackageScope::ConnStringDB]");
            OLEDBConn.CM.Name = "ExtractDatabase";
            OLEDBConn.CM.DelayValidation = true;

            //Package Sequence
            var Sequence = new EzSequence(package);
            Sequence.Name = "SSISSampleSequence";

            //Package Save
            string path = "C:\\SSISSamplePackage.dtsx";
            package.SaveToFile(path);
        }
    }
}
```