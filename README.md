# nujno
        private void UpdateLust()
        {
            ModelBD.BaseModel.Getcontext().ChangeTracker.Entries().ToList().ForEach(p => p.Reload());
            Lproduct.ItemsSource = ModelBD.BaseModel.Getcontext().Product.ToList();
        }



        private void edit_Click(object sender, RoutedEventArgs e)
        {
            Window1 win = new Window1((sender as Button).DataContext as Product);
                win.Show();
            this.Close();
        }
        
     
        
        
        
    public partial class Window1 : Window
    {
        ModelBD.Product _editproduct = new ModelBD.Product();
        public Window1(Product selectProduct)
        {
            InitializeComponent();
            _editproduct = selectProduct;
            DataContext = _editproduct;
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            ModelBD.BaseModel.Getcontext().Product.Remove(_editproduct);

                ModelBD.BaseModel.Getcontext().SaveChanges();
                MainWindow win = new MainWindow();
                win.Show();
                this.Close();
 
        }
    }
}




















 public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            UpdateTub();
            var filtr = NineBase.Getcontext().ProductType.ToList();
            filtr.Insert(0, new ProductType
            {
                Title = "Все типы"
            });
            Filtr.ItemsSource = filtr;
            Filtr.SelectedIndex = 0;
            SortComb.SelectedIndex = 0;

        }
        private void UpdateTub()
        {
            NineBase.Getcontext().ChangeTracker.Entries().ToList().ForEach(p => p.Reload());
            ListProduct.ItemsSource = NineBase.Getcontext().Product.ToList();
        }
        private void Search()
        {
            var Result = NineBase.Getcontext().Product.ToList();
            if (Filtr.SelectedIndex > 0)
                Result = Result.Where(p => Convert.ToString(p.ProductTypeID).Contains(Convert.ToString(Filtr.SelectedValue))).ToList();
            if (SortComb.SelectedIndex == 1)
                Result = Result.OrderBy(p => p.Title).ToList();
            if (SortComb.SelectedIndex == 2)
                Result = Result.OrderByDescending(p => p.Title).ToList();
            if (SortComb.SelectedIndex == 3)
                Result = Result.OrderBy(p => p.ProductionWorkshopNumber).ToList();
            if (SortComb.SelectedIndex == 4)
                Result = Result.OrderByDescending(p => p.ProductionWorkshopNumber).ToList();
            if (SortComb.SelectedIndex == 5)
                Result = Result.OrderBy(p => p.MinCostForAgent).ToList();
            if (SortComb.SelectedIndex == 6)
                Result = Result.OrderByDescending(p => p.MinCostForAgent).ToList();

            Result = Result.Where(p => p.Title.ToLower().Contains(SearchInput.Text.ToLower()) || p.Description.ToLower().Contains(SearchInput.Text.ToLower())).ToList();


            ListProduct.ItemsSource = Result.ToList();

        }

