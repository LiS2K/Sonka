      public Form1()
        {
            InitializeComponent();
        }
        
        
        
        
        
        
        public double[] array;
        public void Fwrite(string path, decimal rangeBegin, decimal rangeEnd, decimal rangeStep)
        {
            FileInfo fileInfo = new FileInfo(path);
             if (!fileInfo.Exists)
                File.Create(path).Close();
            //if (fileInfo.Exists)
           // {
                System.IO.File.WriteAllText(path, string.Empty);
                int j = 0;
                FileStream fs = new FileStream(fileInfo.FullName, FileMode.Open);
                StreamWriter sw = new StreamWriter(fs);
                sw.BaseStream.Position = 0;
                Calculation(rangeBegin, rangeEnd, rangeStep, out array);
                for (decimal i = rangeBegin; i <= rangeEnd; i+=rangeStep)
                {
                    sw.WriteLine("f({0}) = " + array[j++], i.ToString());
                }
                sw.Close();
            //}
           // else
            //{
                //File.Create(path);
            //}
        }
        public void Calculation(decimal spanBegin, decimal spanEnd, decimal rangeStep, out double[] array)
        {
            int k = 0;
            array = new double[(int)(spanEnd/ rangeStep)];
            for (decimal i = spanBegin; i <= spanEnd; i += rangeStep)
            {
                array[k++] = Math.Cos(Convert.ToDouble(i)) * Convert.ToDouble(i);
            }
        }
        private void button1_Click(object sender, EventArgs e)
        {
            Fwrite(textBox1.Text, rangeBegin.Value, rangeEnd.Value, Step.Value);
        }

        private void button2_Click(object sender, EventArgs e)
        {
            FileInfo fileInfo = new FileInfo(Directory.GetCurrentDirectory() + "\\Data.txt");
            if (fileInfo.Exists)
            { 
            string fileName = Directory.GetCurrentDirectory()+ "\\Data.txt";
            var lines = File.ReadAllLines(fileName);
            for (int i = 0; i < lines.Count(); i++)
            {
                while (lines[i].Contains("  ")) { lines[i] = lines[i].Replace("  ", " "); }
                lines[i] = lines[i].Split(' ').OrderBy(g => g.Length).Aggregate((q, w) => q + " " + w);
            }
            richTextBox1.Text += string.Join(Environment.NewLine, lines);
            }
            else
            {
              richTextBox1.Text += "Такого файла не существует";
            }
        }
    }
}
