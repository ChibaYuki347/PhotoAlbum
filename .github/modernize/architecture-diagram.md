# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using SQL Server LocalDB for metadata persistence and local file system storage for uploaded images.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\n(HTTP Client)"]

    subgraph WebApp["ASP.NET Core 9.0 Web Application"]
        subgraph Pages["Razor Pages (Presentation Layer)"]
            IndexPage["Index.cshtml\n(Gallery Grid + Upload)"]
            DetailPage["Detail.cshtml\n(Full-size View + Metadata)"]
            PhotoFilePage["PhotoFile.cshtml\n(File Retrieval Endpoint)"]
        end

        subgraph Services["Service Layer"]
            IPhotoService["IPhotoService\n(Interface)"]
            PhotoService["PhotoService\n(Upload, Validate, Retrieve, Delete)"]
            ImageSharp["SixLabors.ImageSharp 3.x\n(Image Dimension Extraction)"]
        end

        subgraph Data["Data Access Layer"]
            EFCore["Entity Framework Core 9.0\n(ORM)"]
            PhotoAlbumContext["PhotoAlbumContext\n(DbContext)"]
            PhotoModel["Photo Model\n(Filename, Size, MIME, Dimensions, Timestamp)"]
        end
    end

    subgraph Storage["Data Storage"]
        SQLServer["SQL Server LocalDB\n(Photo Metadata)"]
        FileSystem["Local File System\nwwwroot/uploads\n(Image Files - GUID-named)"]
    end

    Browser -->|"HTTP Requests"| Pages
    Pages -->|"Calls"| IPhotoService
    IPhotoService -->|"Implemented by"| PhotoService
    PhotoService -->|"Uses"| ImageSharp
    PhotoService -->|"Persists metadata via"| EFCore
    EFCore -->|"Maps"| PhotoAlbumContext
    PhotoAlbumContext -->|"Uses"| PhotoModel
    PhotoAlbumContext -->|"Connects to"| SQLServer
    PhotoService -->|"Stores image files"| FileSystem
    PhotoFilePage -->|"Serves files from"| FileSystem
```
