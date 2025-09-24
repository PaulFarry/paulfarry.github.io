deleting extended properties from sql server based on category

DECLARE @sql NVARCHAR(MAX) = N'';

SELECT @sql += '
EXEC sp_dropextendedproperty 
    @name = N''notes'', 
    @level0type = N''SCHEMA'', @level0name = ''' + s.name + ''',
    @level1type = N''TABLE'', @level1name = ''' + o.name + ''',
    @level2type = N''COLUMN'', @level2name = ''' + c.name + ''';'
FROM sys.extended_properties ep
JOIN sys.objects o ON ep.major_id = o.object_id
JOIN sys.schemas s ON o.schema_id = s.schema_id
LEFT JOIN sys.columns c ON ep.minor_id = c.column_id AND c.object_id = o.object_id
WHERE ep.name = 'notes';

EXEC sp_executesql @sql;


