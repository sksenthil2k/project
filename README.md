$outputFile = "project_dump.txt"

tree /F /A | Out-File $outputFile

Get-ChildItem . -Recurse -File |
Where-Object {
    $_.FullName -notmatch "\\target\\" -and
    $_.FullName -notmatch "\\.git\\" -and
    $_.FullName -notmatch "\\.idea\\"
} |
ForEach-Object {

    "`n`n==================================================" | Out-File $outputFile -Append
    "FILE: $($_.FullName)" | Out-File $outputFile -Append
    "==================================================" | Out-File $outputFile -Append

    try {
        Get-Content $_.FullName -ErrorAction Stop | Out-File $outputFile -Append
    }
    catch {
        "[BINARY FILE]" | Out-File $outputFile -Append
    }
}
