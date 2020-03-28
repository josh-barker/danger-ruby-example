# Validate Branch
if github.branch_for_head.split('/').size == 1
  failure("Branch name should have group at the beginning. For example, feature/some-feature. Your branch name is: '#{github.branch_for_head}'")
end

# Validate Commit Messages
git.commits.each do |c|
  if c.message.split.size <= 2
    failure("Do not have commit messages with less than 2 words - '#{c.message}'")
  end

  if c.message =~ /^Merge branch/ 
    fail('Please rebase to get rid of the merge commits in this PR')
  end
end

# Validate PR title

if github.pr_title =~ /^\w+\//i
  failure("PR title should not include the branch group - '#{github.pr_title}'")
end

# Validate PR Body
if github.pr_body =~ /<!--.*-->/
  failure("PR body must be filled in!")
end

# Validate ticket from workflow tool. For example, Jira
unless github.pr_body.include?("htts://jira.com/blah")
  failure("Missing Jira Ticket Link")
end

# Validate all steps have been completed
lines_with_checkboxes = github.pr_body.lines.select {|l| l =~ /^\s*\-\s+\[\s+\]/ }.map(&:strip)
lines_with_checkboxes.each do |line|
  # ignore items that have a strike through
  # for example:
  # - [ ] ~not applicable~
  unless line =~ /~.*~/
    failure("You need to complete #{line}")
  end
end
