# Test-
Test
Кнопка перехода
this.Hide();   
Form2 form2 = new Form2();
form2.Show();

Сортировать кнопкаprivate void button1_Click(object sender, EventArgs e)
        {
            System.Windows.Forms.DataGridViewColumn Col; Col = фИОDataGridViewTextBoxColumn;
                switch (listBox1.SelectedIndex)
            {
                case 0: Col = фИОDataGridViewTextBoxColumn;
                    break;
                case 1: Col = кодСотрудниковDataGridViewTextBoxColumn; 
                    break;
            }
            if(radioButton1.Checked)
            {
                dataGridView1.Sort(Col, System.ComponentModel.ListSortDirection.Ascending);
            }
            else
            {
                dataGridView1.Sort(Col, System.ComponentModel.ListSortDirection.Descending);
            }
        }

Рис. 16 Упрощение кода
using System;
using System.Data.SqlClient;

namespace Cafe
{
    public partial class Form4 : Form
    {
        private String tbl;

        private SqlConnection conn;

        private SqlCommand cmd;

        public Form4()
        {
            InitializeComponent();
            StartPosition = FormStartPosition.CenterScreen;

            conn = new SqlConnection("Data Source=192.168.10.20;Initial Catalog=user35;User ID=user35;Password=34177");
            cmd = new SqlCommand("insert into Заказ values (@НомерСтопика, @Первое, @Второе, @Напитки, @ДатаЗаказы, @СтатусЗаказа, @Готовка)", conn);

            FormClosed += new FormClosedEventHandler(Form4_FormClosed);
        }

        private void button13_Click(object sender, EventArgs e)
        {
            // Кнопка добавить

            conn.Open();

            cmd.Parameters.AddWithValue("@НомерСтопика", textBox4.Text);
            cmd.Parameters.AddWithValue("@Первое", textBox1.Text);
            cmd.Parameters.AddWithValue("@Второе", textBox3.Text);
            cmd.Parameters.AddWithValue("@Напитки", textBox7.Text);
            cmd.Parameters.AddWithValue("@ДатаЗаказы", textBox8.Text);
            cmd.Parameters.AddWithValue("@СтатусЗаказа", textBox6.Text);
            cmd.Parameters.AddWithValue("@Готовка", textBox5.Text);

            cmd.ExecuteNonQuery();

            // Обновляем таблицу
            this.заказыTableAdapter.Fill(this.user35DataSet.Заказы);

            // Закрываем соединение
            conn.Close();
        }

        private void Form4_FormClosed(object sender, FormClosedEventArgs e)
        {
            if (conn != null)
            {
                conn.Dispose();
            }
        }
    }
}

Рис. 17 Код кнопки Добавить
Рис. 18 Код кнопки Удалить

Рис. 19 Код кнопки Редактировать

Рис. 20 Код кнопки Обновить

Авторизация
private const string connectionString = "Data Source=192.168.10.20;Initial Catalog=user35;User ID=user35;Password=34177";
        private const string commandText = @"SELECT name_role from sp_role, account 
                                           WHERE Login = @login AND Password = @password AND account.id_role=sp_role.id_role";string login = textBox1.Text;
            string password = textBox2.Text;

            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                connection.Open();

                using (SqlCommand command = new SqlCommand(commandText, connection))
                {
                    command.Parameters.AddWithValue("@login", login);
                    command.Parameters.AddWithValue("@password", password);

                    string roleName = (string)command.ExecuteScalar();
                    if (roleName != null)
                    {
                        // Переход на нужную форму в соответствии с ролью 
                        switch (roleName)
                        {
                            case "Admin":
                                Form2 form2 = new Form2();
                                form2.Show();
                                this.Hide();
                                break;
                            //case "Cook":
                            //    Form4 form4 = new Form4();
                            //    form4.Show();
                            //    this.Hide();
                            //    break;
                            //case "Waiter":
                            //    Form4 form5 = new Form4();
                            //    form5.Show();
                            //    this.Hide();
                            //    break;
                            default:
                                MessageBox.Show("Неверное имя роли");
                                break;
                        }
                    }
                    else
                    {
                        MessageBox.Show("Неверный логин или пароль");
                    }
                }
            }

2.private const string connectionString = "Data Source=192.168.10.20;Initial Catalog=user35;User ID=user35;Password=34177";

string login = textBox1.Text;
string password = textBox2.Text;

using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();

    // SQL-запрос для проверки подлинности и получения роли
    using (SqlCommand command = new SqlCommand("SELECT role FROM users WHERE login = @login AND password = @password", connection))
    {
        command.Parameters.AddWithValue("@login", login);
        command.Parameters.AddWithValue("@password", password);

        string roleName = (string)command.ExecuteScalar();

        if (roleName != null)
        {
            // Переход на соответствующую форму
            if (roleName == "Admin")
            {
                Form2 form2 = new Form2();
                form2.Show();
                this.Hide();
            }
            else if (roleName == "Cook")
            {
                Form4 form4 = new Form4();
                form4.Show();
                this.Hide();
            }
            else if (roleName == "Waiter")
            {
                Form5 form5 = new Form5();
                form5.Show();
                this.Hide();
            }
            else
            {
                MessageBox.Show("Неверная роль пользователя: " + roleName);
            }
        }
        else
        {
            MessageBox.Show("Неверный логин или пароль");
        }
    }
}

Кнопка фильтр
заказыBindingSource.Filter = "НомерСтопика='" + this.comboBox1.Text + "'";
Кнопка обновить
заказыBindingSource.Filter = "";
Кнопка найти
{
                int i = 0;
                int j = 0; for (i = 0; i < dataGridView1.RowCount; i++)
                {
                    for (j = 0; j < dataGridView1.ColumnCount; j++)
                    {
                        dataGridView1.Rows[i].Cells[j].Style.BackColor = Color.White;
                        dataGridView1.Rows[i].Cells[j].Style.ForeColor = Color.Black;
                    }
                }; for (i = 0; i < dataGridView1.RowCount; i++)
                {
                    for (j = 0; j < dataGridView1.ColumnCount; j++)
                    {
                        if (dataGridView1.Rows[i].Cells[j].Value == null)
                        {
                            break;
                        }
                        if (textBox2.Text == dataGridView1.Rows[i].Cells[j].Value.ToString())
                        {
                            dataGridView1.Rows[i].Cells[j].Style.BackColor = Color.AliceBlue; dataGridView1.Rows[i].Cells[j].Style.ForeColor = Color.Blue;
                        }
                    }
                };
            }
Команда для центра экрана
StartPosition = FormStartPosition.CenterScreen;
