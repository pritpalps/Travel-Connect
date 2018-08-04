# Travel-Connect
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Firstweek
{
    public partial class Login : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void btnLogin_Click(object sender, EventArgs e)
        {
            if (isValid())
                Response.Redirect("Profile.aspx");

        }

        private bool isValid()
        {
            bool valid = false;
            DataSet userDS = GetUserInfo(txtUserName.Text, txtPassword.Text);
            if (userDS != null && userDS.Tables.Count > 0 && userDS.Tables[0].Rows.Count > 0)
            {
                valid = true;
            }
            return valid;
        }

        private DataSet GetUserInfo(string userName, string password)
        {
            DataSet ds = new DataSet();
            string connectionString = "Data Source=laptop-qbicv938\\mysqlserver1;Initial Catalog=Firstweek;Integrated Security=True";

            var queryString = "SELECT * FROM USERS WHERE USERNAME = '" + txtUserName.Text + "' AND PASSWORD = '" + txtPassword.Text;
            using (SqlConnection connection = new SqlConnection(
              connectionString))
            {
                SqlCommand command = new SqlCommand(queryString);
                SqlDataAdapter da = new SqlDataAdapter(queryString, connection);
                da.Fill(ds);
                if (connection.State == ConnectionState.Open)
                {
                    connection.Close();
                }
            }

            return ds;
        }


    }
}
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Firstweek
{
    public partial class Register : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void btnRegister_Click(object sender, EventArgs e)
        {
            lblMsg.Visible = false;
            int userId = SaveUser();
            if (userId <=0)
            {
                lblMsg.Visible = true;
                lblMsg.Text = "Something went wrong please try again...";
            }
            else
            {
                Response.Redirect("profile.aspx?uid=" + userId.ToString());
            }
        }

        private int SaveUser()

        {
            int userId = 0;
            try
            {
                string connectionString = "Data Source=laptop-qbicv938\\mysqlserver1;Initial Catalog=Firstweek;Integrated Security=True";

                var queryString = "INSERT INTO USERS (USERNAME, PASSWORD, FIRSTNAME, LASTNAME, EMAILADDRESS, AGE, GENDER, TYPESOFTRAVEL)" +
                    " VALUES ('" + txtUserName.Text + "', '" + txtPassword.Text + "', '" + txtFirstName.Text + "', '" + txtLastName.Text + "', '" + txtEMail.Text + "', 30 , '" + ddlGender.SelectedValue.ToString() + "', '" + ddlTravelTypes.SelectedValue.ToString() + "'); SELECT SCOPE_IDENTITY();";
                using (SqlConnection connection = new SqlConnection(
                  connectionString))
                {
                    if (connection.State == ConnectionState.Closed)
                    {
                        connection.Open();
                    }
                    SqlCommand command = new SqlCommand(queryString, connection);

                   // cmd.ExecuteNonQuery();

                   var uId = command.ExecuteScalar();
                    if (uId != null)
                        userId = int.Parse(uId.ToString());
                    connection.Close();
                    connection.Dispose();
                }
            }
            catch (Exception ex)
            {
                userId = -1;
            }
            return userId;
        }
    }
}using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace Firstweek
{
    public partial class profile : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }
    }
}
