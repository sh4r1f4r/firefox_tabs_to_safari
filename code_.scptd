-- Save all URLs from open Firefox tabs
set tabURLs to {}

tell application "System Events"
    -- Activate Firefox
    tell application "Firefox" to activate
    delay 0.5 -- Delay to ensure Firefox is active

    -- Check if the Firefox process is available
    if not (exists process "Firefox") then
        display dialog "Firefox is not running. Please launch Firefox and try again." buttons {"OK"} default button "OK"
        return
    end if

    tell process "Firefox"
        -- Retrieve URLs from all tabs
        set firstTabURL to ""
        repeat
            set currentURL to ""
            -- Focus on the address bar
            keystroke "l" using {command down}
            delay 0.2
            -- Copy the URL to the clipboard
            keystroke "c" using {command down}
            delay 0.2
            try
                set currentURL to the clipboard as text
            on error
                display dialog "Failed to retrieve the URL of the current tab. Please check clipboard access." buttons {"OK"} default button "OK"
                return
            end try

            -- Check and add the URL
            if currentURL is not "" and currentURL is not in tabURLs then
                set end of tabURLs to currentURL
            end if

            -- Save the first URL to check when returning to the first tab
            if firstTabURL is "" then
                set firstTabURL to currentURL
            else if currentURL = firstTabURL then
                exit repeat -- Returning to the first tab ends the loop
            end if

            -- Move to the next tab
            keystroke "]" using {command down, shift down}
            delay 0.2
        end repeat
    end tell
end tell

-- Open URLs in Safari
if tabURLs is {} then
    display dialog "No URLs were retrieved from Firefox. Please ensure tabs are open." buttons {"OK"} default button "OK"
else
    tell application "Safari"
        activate
        -- Create a new Safari window
        set safariWindow to make new document
        delay 0.5 -- Delay to ensure the window is created

        -- Open URLs in new tabs
        repeat with i from 1 to count of tabURLs
            set tabURL to item i of tabURLs
            if i = 1 then
                -- The first tab is opened in the current tab
                set URL of front document to tabURL
            else
                -- All other tabs are opened as new tabs
                tell window 1
                    set newTab to make new tab
                    set URL of newTab to tabURL
                end tell
            end if
            delay 0.5 -- Delay between opening tabs
        end repeat
    end tell
    display dialog "All tabs have been successfully transferred to Safari!" buttons {"OK"} default button "OK"
end if
