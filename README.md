# BlazorContosoUniversity

PMC:
Install-Package Microsoft.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore

In data folder add Student.cs
	[Table("Students", Schema = "dbo")]
    public class Student
    {
        public int ID { get; set; }
		[MaxLength(20, ErrorMessage = "to long"), MinLength(2, ErrorMessage = "to short")]
        public string LastName { get; set; }
		[MaxLength(20, ErrorMessage = "to long"), MinLength(2, ErrorMessage = "to short")]
        public string FirstMidName { get; set; }
        public DateTime EnrollmentDate { get; set; }

        public ICollection<Enrollment> Enrollments { get; set; }
    }
    
	In data folder add Enrollment.cs
	public enum Grade
    {
        A, B, C, D, F
    }
    public class Enrollment
    {
        public int EnrollmentID { get; set; }
        public int CourseID { get; set; }
        public int StudentID { get; set; }
        [DisplayFormat(NullDisplayText = "No grade")]
        public Grade? Grade { get; set; }

        public Course Course { get; set; }
        public Student Student { get; set; }
    }

In data folder add Course.cs
    public class Course
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int CourseID { get; set; }
		[MaxLength(20, ErrorMessage = "to long"), MinLength(2, ErrorMessage = "to short")]
        public string Title { get; set; }
        public int Credits { get; set; }

        public ICollection<Enrollment> Enrollments { get; set; }
    }
	
add in Data class to our application
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {
    }
	  public DbSet<Student> Students { get; set; }
    public DbSet<Enrollment> Enrollments { get; set; }
    public DbSet<Course> Courses { get; set; }
}

In your appsettings.json file, add in the following
"ConnectionStrings": {
    "DataConnectionString": "Data Source=(localdb)\\mssqllocaldb;Database=BlazorContosoUniversity;;Trusted_Connection=True;MultipleActiveResultSets=true"
  }

In the Program.cs bebore var app = builder.Build(); add:
builder.Services.AddDbContextFactory<ApplicationDbContext>(options =>
   options.UseSqlServer(builder.Configuration.GetConnectionString("DataConnectionString"), sqlServerOptions => sqlServerOptions.CommandTimeout(120)),
   ServiceLifetime.Transient
);
			

PMC:
add-migration initial
update-database
