# BlazorContosoUniversity
Create Blazor Server project: BlazorContosoUniversity
Run command in Package Nabager Console(PMC):
Install-Package Microsoft.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore

In data folder add Student.cs
In data folder add Course.cs
In data folder add Enrollment.cs

In data folder add ApplicationDbContext.cs
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

In shared folder add DBContextPage.razor

@using BlazorContosoUniversity.Data
@using Microsoft.EntityFrameworkCore
@inject IDbContextFactory<ApplicationDbContext> DbFactory
@code {
    protected ApplicationDbContext dbContext;
    protected override async Task OnInitializedAsync()
    {
        await base.OnInitializedAsync();
        if (dbContext == null)
        {
            dbContext = await DbFactory.CreateDbContextAsync();
        }
    }
}

In Page folder create pages:

