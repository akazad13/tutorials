        private List<ContactPersonModel> SearchDataFromExcel(string name)
        {
            var excelData = new List<ContactPersonModel>();
            try
            {
                var filePath = Server.MapPath("~/ExcelDataset/EmployeeMaster.xlsx");
                var file = new FileInfo(filePath);
                var connString = string.Format("Provider=Microsoft.ACE.OLEDB.12.0; Data Source={0};Extended Properties=\"Excel 12.0 Xml;HDR=Yes;IMEX=1\"", filePath);

                var dtExcel = new DataTable();

                using (var conn = new OleDbConnection(connString))
                {
                    conn.Open();
                    var tableschema = conn.GetSchema("Tables");
                    var firstsheet = tableschema.Rows[0]["TABLE_NAME"].ToString();
                    string name_query = "SELECT [Contact Person], [Employee Code], [Department] FROM [" + firstsheet + "] WHERE [Contact Person] LIKE @ContactPerson";
                    var cmd = new OleDbCommand(string.Format(name_query), conn);
                    cmd.Parameters.Add("@ContactPerson", OleDbType.VarChar).Value = "%" + name + "%";
                    var da = new OleDbDataAdapter(cmd);
                    da.Fill(dtExcel);
                }

                foreach (DataRow row in dtExcel.Rows)
                {
                    int totalColumn = row.ItemArray.Length;
                    var arr = row.ItemArray;

                    excelData.Add(new ContactPersonModel
                    {
                        ContactPerson = arr[0].ToString(),
                        EmployeeCode = arr[1].ToString(),
                        Department = arr[2].ToString()
                    });
                }
            }
            catch (Exception ex)
            {
                throw new Exception(ex.Message);
            }

            return excelData;
        }

        private ContactPersionDetailsModel GetContactPersionData(string code)
        {
            var excelData = new ContactPersionDetailsModel();
            try
            {
                var filePath = Server.MapPath("~/ExcelDataset/EmployeeMaster.xlsx");
                var file = new FileInfo(filePath);
                var connString = string.Format("Provider=Microsoft.ACE.OLEDB.12.0; Data Source={0};Extended Properties=\"Excel 12.0 Xml;HDR=Yes;IMEX=1\"", filePath);

                var dtExcel = new DataTable();

                using (var conn = new OleDbConnection(connString))
                {
                    conn.Open();
                    var tableschema = conn.GetSchema("Tables");
                    var firstsheet = tableschema.Rows[0]["TABLE_NAME"].ToString();
                    string name_query = "SELECT * FROM [" + firstsheet + "] WHERE [Employee Code] =  @Code";
                    var cmd = new OleDbCommand(string.Format(name_query), conn);
                    cmd.Parameters.Add("@Code", OleDbType.VarChar).Value = code;
                    var da = new OleDbDataAdapter(cmd);
                    da.Fill(dtExcel);
                }

                foreach (DataRow row in dtExcel.Rows)
                {
                    int totalColumn = row.ItemArray.Length;
                    var arr = row.ItemArray;

                    excelData = new ContactPersionDetailsModel
                    {
                        ContactPerson = arr[1]?.ToString(),
                        EmployeeCode = arr[2]?.ToString(),
                        Designation = arr[3]?.ToString(),
                        ContactNumber = arr[5]?.ToString(),
                        Email = arr[6]?.ToString(),
                        Location = arr[7]?.ToString()
                    };
                }
            }
            catch (Exception ex)
            {
                throw new Exception(ex.Message);
            }

            return excelData;
        }
