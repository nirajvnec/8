var mappedResult = result.Select(col => new TableElectroColumnInfo
{
    Name = col.Name,
    IsRequired = col.IsRequired,
    Length = col.Length,
    // Map type
    Type = !string.IsNullOrEmpty(col.Type)
        ? (col.Type.ToLower().Contains("int") ||
           col.Type.ToLower().Contains("decimal") ||
           col.Type.ToLower().Contains("numeric") ||
           col.Type.ToLower().Contains("float") ||
           col.Type.ToLower().Contains("real"))
            ? "number"
        : (col.Type.ToLower().Contains("date") ||
           col.Type.ToLower().Contains("datetime") ||
           col.Type.ToLower().Contains("smalldatetime"))
            ? "date"
        : (col.Type.ToLower().Contains("varchar") ||
           col.Type.ToLower().Contains("nvarchar") ||
           col.Type.ToLower().Contains("char") ||
           col.Type.ToLower().Contains("nchar") ||
           col.Type.ToLower().Contains("text"))
            ? "string"
        : (col.Type.ToLower() == "bit")
            ? "boolean"
        : col.Type
        : col.Type
}).ToList();