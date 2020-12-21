

namespace BanasButter_WebAPI.Controllers
{
    public class SSController : ApiController
    {
        SqlDbHelper SqlDbHelper = new SqlDbHelper();
        Global_Fun Global = new Global_Fun();
        [HttpGet]
        [ResponseType(typeof(List<SimpliStock>))]
        public IHttpActionResult SimpliStock_Records()
        {
            DataTable DataTable = new DataTable();
            Global_Fun Global_Fun = new Global_Fun();
            //LstSimpleS LstSimpleS = new LstSimpleS();

            List<SimpliStock> LstSimpliStock = new List<SimpliStock>();

            try
            {

                string query = "select convert(varchar,dateadd(ss,5,max(Sdate)),120)as fromdate from B3_BUTTER_API_SimpliStock";
                DataTable = SqlDbHelper.GetDS(query, "B3_BUTTER_API_SimpliStock", false, DateTime.Now.AddMinutes(1));

                if (DataTable.Rows.Count > 0)
                {
                    string fquery = "select convert(varchar,dateadd(n,1, max(Sdate)), 120)  as 'fromdate' from B3_BUTTER_API_SimpliStock";
                    DataTable = SqlDbHelper.GetDS(fquery, "tbl_BanasLMPTOTPD_TRANSFER_API", false, DateTime.Now.AddMinutes(1));
                    string strFromDate = DataTable.Rows[0][0].ToString();
                    // Global_Fun.ErrorLog("GetCIPRecord::FromDate", "Log Message");

                    query = "select convert(varchar,max(t_stamp),120)as Edate from B3_BUTTER_DL_SimliStock";
                    DataTable = SqlDbHelper.GetDS(query, "B3_BUTTER_DL_SimliStock", false, DateTime.Now.AddMinutes(1));

                    //if (DataTable.Rows.Count > 0)
                    string strToDate = DateTime.Now.ToString("yyyy/MM/dd HH:mm:ss");
                    //Global_Fun.ErrorLog("GetCIPRecord:toDate", "Log Message"



                    string rptquery = "select isnull(TypeOfBoxes,'') as TypeOfBoxes, isnull(NoOfBoxes,0) as NoOfBoxes, isnull(AvgWeight,0) as AvgWeight, isnull(SDeviation,0) as SDeviation, isnull(MaximumDiviation,0) as MaximumDiviation, isnull(MinDiviation,0) as MinDiviation, isnull(SDate,'') as SDate, isnull(EDate,'') as EDate, AvgQty=(NoOfBoxes)/( case when (DateDiff (hh,(SDate), (EDate) ))=0 then 1 else DateDiff (hh,(SDate) , (EDate) )end),TotalQty=((AvgWeight)*(NoOfBoxes)/1000),isnull(TotalDiviation,0) as TotalDiviation,Id from B3_BUTTER_API_SimpliStock where API_Status=0 order by Id Asc";
                    DataTable = SqlDbHelper.GetDS(rptquery, "IDMC_SimpliStock.Reprot_BN-", false, DateTime.Now.AddMinutes(1.0f));
                    if (DataTable.Rows.Count > 0)
                    {
                        for (int i = 0; i <= DataTable.Rows.Count - 1; i++)
                        {

                            SimpliStock SimpliStockMaster = new SimpliStock();
                            SimpliStockMaster.TypeOfBoxes = DataTable.Rows[i]["TypeOfBoxes"].ToString();
                            SimpliStockMaster.NoOfBoxes = Convert.ToInt32(DataTable.Rows[i]["NoOfBoxes"].ToString());
                            SimpliStockMaster.AvgWeight = Convert.ToSingle(DataTable.Rows[i]["AvgWeight"].ToString());
                            SimpliStockMaster.SDeviation = Convert.ToSingle(DataTable.Rows[i]["SDeviation"].ToString());
                            SimpliStockMaster.MaximumDiviation = Convert.ToSingle(DataTable.Rows[i]["MaximumDiviation"].ToString());
                            SimpliStockMaster.MinDiviation = Convert.ToSingle(DataTable.Rows[i]["MinDiviation"].ToString());
                            SimpliStockMaster.SDate = Convert.ToDateTime(DataTable.Rows[i]["SDate"].ToString());
                            SimpliStockMaster.EDate = Convert.ToDateTime(DataTable.Rows[i]["EDate"].ToString());
                            SimpliStockMaster.AvgQty = Convert.ToSingle(DataTable.Rows[i]["AvgQty"].ToString());
                            SimpliStockMaster.TotalQty = Convert.ToSingle(DataTable.Rows[i]["TotalQty"].ToString());
                            SimpliStockMaster.TotalDiviation = Convert.ToSingle(DataTable.Rows[i]["TotalDiviation"].ToString());
                            SimpliStockMaster.Ack_Id = Convert.ToInt32(DataTable.Rows[i]["Id"].ToString());

                            LstSimpliStock.Add(SimpliStockMaster);


                        }
                        //HomeWrapper.Result = "Success";
                        //LstSimpleS.LstSimpliStock = LstSimpliStock;
                    }
                    else
                    {
                        //HomeWrapper.Result = "NO DATA FOUND";

                    }
                }
                return Ok(LstSimpliStock);
            }
            catch (Exception ex)
            {
                //HomeWrapper.Result = ex.ToString();
                return Ok(LstSimpliStock);
            }
            }
        }

    }

